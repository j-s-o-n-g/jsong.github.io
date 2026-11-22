---
title: "GCP Lab: Analyzing WTA Match Data with Hadoop MapReduce"
date: 2026-11-22
categories: [Cloud]
tags: [gcp, cloud-computing, hadoop, mapreduce, docker, hdfs, java, data-engineering]
comments: true
---

As part of **CSE 3244: Data Management in the Cloud** at The Ohio State University, I used Google Cloud Platform (GCP) to provision a Linux virtual machine and run a Hadoop MapReduce analysis against Women's Tennis Association (WTA) match data. The lab connected cloud infrastructure fundamentals with a complete, command-line data processing workflow: provision compute, prepare a containerized Hadoop environment, load source files into HDFS, compile a Java job, and interpret its output.

The analysis counted match wins by player and the country associated with each tournament. I also changed the MapReduce key to report player names instead of IDs, making the aggregate easier to interpret.

## Lab Architecture

```text
GCP project
└── Compute Engine VM (Ubuntu 20.04, 4 vCPU, 16 GB RAM, 50 GB disk)
    └── Docker container: Cloudera QuickStart
        ├── Hadoop and HDFS
        └── Java MapReduce job

Host directory ~/tmp  ── mounted at ──>  container directory /src
Match CSV files       ── uploaded to ──> HDFS directory /tennis
MapReduce output       ── written to ──> HDFS directory /tennis.CountWins
```

The VM provided the cloud compute environment. Docker made the preconfigured Hadoop distribution available without a manual cluster installation, and a bind mount exposed the host's working directory inside the container. HDFS remained a separate storage layer: files on the VM had to be explicitly uploaded before Hadoop jobs could read them.

## Provisioning the Environment

I created a GCP project and an Ubuntu 20.04 LTS Compute Engine VM, then connected through the browser-based SSH terminal. The lab used an `e2-standard-4` machine (4 vCPUs and 16 GB of memory) and a 50 GB balanced persistent disk. On Ubuntu, I installed Docker and downloaded the Cloudera QuickStart image, which was several gigabytes in size.

The container was launched with the lab directory mounted into it:

```bash
sudo docker run \
  --hostname=quickstart.cloudera \
  --privileged=true \
  --interactive --tty \
  --volume="$HOME/tmp:/src" \
  <quickstart-image-id> \
  /usr/bin/docker-quickstart -d
```

I used `docker images` to inspect the downloaded image and `docker ps` to find the running container. The lab also covered detaching from a running container with `Ctrl+P`, then `Ctrl+Q`, and reconnecting with `docker attach`.

## Loading the Match Data into HDFS

After downloading and unpacking the lab files under `~/tmp`, I created an HDFS directory and uploaded the WTA match CSV files:

```bash
hdfs dfs -mkdir /tennis
hdfs dfs -put wta_matches_qual_itf_* /tennis
hdfs dfs -ls /tennis
```

This step made the data available to Hadoop. A file being present in the VM's local filesystem—or in the container's mounted `/src` directory—did not automatically place it in HDFS.

## Building and Running the MapReduce Job

The `CountWins` Java program processed each match record. Its mapper extracted the tournament identifier from the first CSV column, used the fourth hyphen-separated segment as the tournament country when available, and emitted the winner and country as a composite key with a value of `1`. Records sharing a key were then summed to produce the number of wins.

The job was compiled with Hadoop's bundled Java compiler, packaged into a JAR, and submitted from the command line:

```bash
hadoop com.sun.tools.javac.Main -Xlint:deprecation CountWins.java
jar cf cw.jar CountWins*.class

# Hadoop requires the output directory not to exist before a job runs.
hdfs dfs -rm -r /tennis.CountWins
hadoop jar cw.jar CountWins /tennis /tennis.CountWins

hdfs dfs -cat /tennis.CountWins/part-r-00000 | head -n 20
```

The output key determined what the aggregation meant. For the bonus analysis, I changed the mapper to use the winner's name (the eleventh CSV field) instead of the player ID. The reducer could then aggregate by **player name and country** with the same counting logic.

## Results and Interpretation

The original job produced records in the form `player ID, country, wins`. For example, `200002,USA,63` means the player identified as `200002` won 63 matches at tournaments whose identifier indicated the United States. In the first 20 output rows shown in my lab notes, this was the largest count for that player. When a tournament ID did not contain the expected country segment, the mapper assigned `NA`.

The name-based bonus output made the results more readable. For instance, it showed Abbie Myers with 43 wins associated with Australia in the displayed top 20 rows. These are partial output samples, not a claim that the player led the complete dataset across all players and countries.

## What I Learned

### Cloud resources have a lifecycle and a cost

Disconnecting an SSH session does not stop a VM. Suspending a VM stops its compute activity while retaining its disks, while deleting a VM removes the instance; whether attached disks are also deleted depends on their deletion settings. I learned to check the estimated cost during provisioning and to shut down or remove cloud resources when the work is complete. My notes recorded a console estimate of about **$98.84 per month ($0.14 per hour)** for a 4-vCPU, 16-GB VM with a 10-GB disk. That disk size differs from the 50-GB disk configured in the lab, so the estimate is only a historical console snapshot, not the cost of the exact setup or a current pricing quote.

### Storage layers have explicit boundaries

The workflow involved a host filesystem, a container filesystem view, and HDFS. The bind mount shared files between the host and container, but HDFS still required its own upload step. Keeping track of where data lives is essential when troubleshooting a job that cannot find its input.

### Map and Reduce design starts with the question

The mapper defines the unit being counted by choosing a key. Changing that key from player ID to player name changed the usefulness of the output without changing the basic reduce operation. Domain knowledge about tournament identifiers was also necessary to extract a country code, and the `NA` fallback made missing or unexpected values visible rather than silently dropping them.

### Re-running a batch job requires output management

Hadoop refuses to write to an output path that already exists. Removing the previous output directory before rerunning the job made this behavior explicit and highlighted the need to manage intermediate and result data deliberately.

## Practical Limitations and Production Considerations

This was a teaching lab, not a production deployment. It used a legacy Cloudera QuickStart image and JDK 7, and the container was started with `--privileged=true`. That setting grants broad host-level capabilities and should not be copied into a real deployment without a strong, specific need. A production design would use supported software versions, least-privilege identities and container permissions, managed or appropriately secured compute, and explicit controls for network access, data retention, monitoring, and cleanup.

The mapper shown in the notes splits each record on commas. That is sufficient only when the input fields do not contain CSV-escaped commas or quotes; production data pipelines should use a CSV-aware parser and validate malformed records. The lab's tournament-country extraction also depended on a specific identifier format, so a robust pipeline would validate that assumption and track records that do not match it.

This exercise gave me hands-on practice connecting cloud provisioning, Linux administration, containers, distributed storage, Java, and batch data processing. It also reinforced that an architecture is more than getting a job to run: resource lifecycle, security boundaries, input quality, and output interpretation all matter.
