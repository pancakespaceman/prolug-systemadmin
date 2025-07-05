# Unit 12 Worksheet - Baselines & Benchmarks

## Resources / Important Links

(Kaggle - Python and Data Science Learning)[https://www.kaggle.com/learn]

## Discussion Posts

### 1

Scenario

Your manager has come to you with another emergency.

He has a meeting next week to discuss capacity planning and usage of the system 
with IT upper management. He doesn’t want to lose his budget, but he has to prove 
that the system utilization warrants spending more.

1. What information can you show your manager from your systems?

```
What I need to have to show my manager is long-term usage data from the systems,
at least a week or two. This data would include usage rates of the CPU, Memory,
Disk, and Network as well as number of processes.
I can compare the usage rates of system resources over time to pinpoint
areas that need addressing.
```

2. What type of data would prove system utilization? (Remember the big 4: compute,
memory, disk, networking)

```
CPU
	- CPU percent utilization
	- load average
Memory
	- RAM usage percentage
Disk
	- I/O wait times
	- read/write throughput
	- percentage of storage used
Network
	- bandwith usage
	- package drops
	- errors
```

3. What would your report look like to your manager?

```
Since the report will need to be viewed by non-technical upper management, the
report needs to focus on the main pain points.
Graphs to show where CPU usage is high, or where there are latency spikes, for example.
For each pain point, showcase the issues that arise if they are not addressed.
```

### 2

Scenario:

You are in a capacity planning meeting with a few of the architects. 
They have decided to add 2 more agents to your Linus sytems, Bacula Agent and an Avamar Agent. 
They expect these agents to run their work starting at 0400 every morning.

1. What do these agents do?

```
Both Bacula Agent and Avamar Agent deal with backup and restore operations for
client systems. They are client-server architecture, where a daemon is installed
on each client and a director daemon and storage daemon is installed on a server.
Clients sends backup data to the server based on configuration.
```

2. Do you think there is a good reason not to use these agents at this timeframe?

```
Yes there is good reason to not use these agents at this timeframe.
At 4am, there is an increase in both CPU and RAM usage, with CPU spiking
close to 100%.
Also, the network storage for devices dm-2 and dm-3 are close to being full.
Since Bacular and Avamar would be storing backups on the server, there needs
to be consideration if the server has the space needed.
```

3. Is there anything else you might want to point out to these architects about these
agents they are installing?

```
Both Agents perform overlapping functionality, which means they compete for resources
and could even conflict with each other. 
I would discuss with the architects if there is actually a need for both agents 
or if they can decide on one or the other.
```

### 3

Scenario:

Your team has recently tested at proof of concept of a new storage system. 
The vendor has published the blazing fast speeds that are capable of being run through 
this storage system. You have a set of systems connected to both the old storage system 
and the new storage system.

1. Write up a test procedure of how you may test these two systems.

```
The goal of this test is to test read/write speeds between the existing and new
storage systems. It will use the `fio` tool.
Job files are listed below, taken from 
Oracle Documentation: https://docs.oracle.com/en-us/iaas/Content/Block/References/samplefiocommandslinux.htm

1. Ensure Require tools installed and functioning.
	- `fio`
	- `jq`

2. Create Baseline

If not already gathered, run disk tests on original system to gather baseline
performance metrics.

	- ensure no other users or operations are being performed during testing.
	- Mount original storage system to `/mnt/disktest`
	- RUN: `fio randomReadWrite.fio --output=~/storagetests/originalRandomReadWrite-run1.json`
	- RUN: `fio seqRead.fio --output=~/storagetests/origianlSeqRead-run1.json`
	- unmount `/mnt/disktest`

3. Run Tests on New system

	- ensure no other users or operations are being performed during testing.
	- Mount new storage system to `/mnt/disktest`
	- RUN: `fio randomReadWrite.fio --output=~/storagetests/newRandomReadWrite-run1.json`
	- RUN: `fio seqRead.fio --output=~/storagetests/newSeqRead-run1.json`
	- unmount `/mnt/disktest`

4. Compare metrics between old and new systems.

	- Run the `compareResults.sh` file to output results in termial
	- record the results
```

```randomReadWrite.fio

[global]
bs=4K
iodepth=256
direct=1
ioengine=libaio
group_reporting
time_based
runtime=120
numjobs=4
name=raw-randreadwrite
rw=randrw
output-format=json
							
[job1]
filename=/mnt/disktest
```

```seqRead.fio
[global]
bs=4K
iodepth=256
direct=1
ioengine=libaio
group_reporting
time_based
runtime=120
numjobs=4
name=raw-read
rw=read
output-format=json
							
[job1]
filename=/mnt/disktest
```

```compareResults.sh

cd ~/storagetests/

for file in *.json; do
	echo -n "$file: ";
	jq '.jobs[] | {jobname: .jobname, iops: .read.iops, bw_KBps: .read.bw, lat_ns_mean: .read.lat_ns.mean}' "$file"
done

cd $OLDPWD
```

2. How are you assuring these tests are objective?
	- What is meant by the term Ceteris Paribus in this context?

```
Tests should be run exactly the same way each time. Since we're comparing
two differnt systems, all other possible variables should be constant, such
as running the tests from the same computer with the same parameters, both
storage systems should be mounted with the same procedure.
Ceteris Paribus meaning "all other things being equal" applys in that in order to
show the proper differences between the storage systems, all possible variables
should be the same between tests outside of the one that is being tested for.
```

### Responses

[] Post 1
[] Post 2
[] Post 3

## Definitions

Baseline
	- the documented, approved starting configuration or performance level for a
	system. It serves as a reference point to detect deviations or drift.

Benchmark
	- a standardized set of criteria or tests to evaluate system perormance, compliance,
	or security.

High watermark
	- the maximum observed value of a metric over time -- usefule for capacity
	planning and anomaly detection.

Scope
	- defines the boundaries of what's being evaluated or tested.
		- What systems? (e.g. all RHEL servers)
		- What layers? (e.g. kernel config only? network settings?)
		- What time period?

Methodology
	- the step-by-step approach or strategy used to evaluate a system or perform
	a benchmark.
		- can include tools used, criteria for success, assumptions, etc.

Testing
	- the process of executing checks, scans, or operations to gather data or validate
	assumptioons about a system's behavior or configuration.

Control
	- a specific safegaurd or countermeasure to reduce risk
		- comes from frameworks like NIST, CIS, ISO 27001
		- often numbered (e.g. "CIS Control 1.1.1: Disable unused filesystems")

Experiment
	- testing system behavior under controlled change-- e.g for performance tuning
	or failure mode testing.

Analytics
	- the analysis and interpretation of data collected from systems to derive
	insights, track trends, and support decisions.

## Digging Deeper

1. Analyzing data may open up a new field of interest to you. Go through some of the
free lessons on Kaggle, here: https://www.kaggle.com/learn

	- What did you learn?
	- How will you apply these lessons to data and monitoring you have already
	collected as a system administrator?

2. Find a blog or article that discusses the 4 types of data analytics.

	- What did you learn about past operations?
	- What did you learn about predictive operations?

3. Download Spyder IDE (Open source).

	- Find a blog post or otherwise try to evaluate some data.
	- Perform some Linear regression. My block of code (but this requires some
	additional libraries to be added. I can help with that if you need it.)
	
```python
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
size = [[5.0], [5.5], [5.9], [6.3], [6.9], [7.5]]
price =[[165], [200], [223], [250], [278], [315]]
plt.title('Pizza Price plotted against the size')
plt.xlabel('Pizza Size in inches')
plt.ylabel('Pizza Price in cents')
plt.plot(size, price, 'k.')
plt.axis([5.0, 9.0, 99, 355])
plt.grid(True)
model = LinearRegression()
model.fit(X = size, y = price)
#plot the regression line
plt.plot(size, model.predict(size), color='r')
```

## Reflection Questions

1. What questions do you still have about this week?

2. How can you apply this now in your current role in IT?
If you’re not in IT, how can you look to put something like this into your resume or portfolio?

