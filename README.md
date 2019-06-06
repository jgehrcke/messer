# Messer

This program measures the resource utilization of a specific process at a
specific sampling rate.

In addition to sampling process-specific data, this program also measures the
utilization of system-wide resources making it straightforward to put the
process-specific metrics into context.

Messer values measurement correctness very highly.


## Motivation

This was born out of a need for solid tooling. We started with pidstat from
sysstat, launched in the following manner:

```
pidstat -hud -p $PID 1 1
```

We found that it does not properly account for multiple threads running in the
same process, and that various issues in that regard exist in this program
across various versions. Related references:

- https://github.com/sysstat/sysstat/issues/73#issuecomment-349946051
- https://github.com/sysstat/sysstat/commit/52977c479
- https://github.com/sysstat/sysstat/commit/a63e87996

The program [cpustat](https://github.com/uber-common/cpustat) open-sourced by
Uber has a promising README about the general measurement methodology and
overall seems to be a great tool but the methodology around persisting the
collected timeseries data seems to be fishy and undocumented. It can write a
binary file when using `-cpuprofile` but it is a little unclear what this file
contains and how to analyze the data.

The program [psrecord](https://github.com/astrofrog/psrecord) (which effectively
wraps psutil) has the same fundamental idea as this code here; it however does
not have a clear separation of concerns in the code between persisting the data
to disk, performing the measurements themselves, and plotting the data,
rendering it not production-ready for our concerns.


## Measurands (columns, and their units)

The quantities intended to be measured.

### `proc_io_read_throughput_mibps` and `proc_io_write_throughput_mibps`

Measures the disk I/O throughput of the inspected process, in `MiB/s`.

Based on Linux' `/proc/<pid>/io` `rchar` and `wchar`

> The number of bytes which this task has caused to be read from storage. This
> is simply the sum of bytes which this process passed to read() and pread().
> It includes things like tty IO and it is unaffected by whether or not actual
> physical disk IO was required (the read might have been satisfied from
> pagecache)

Note(JP): rename to `proc_disk_read_throughput_mibps`?

## Notes

- Messer does not asymmetrically hide measurement uncertainty. For example, you
  might see it measure a CPU utilization of a single-threaded process slightly
  larger than 100 %. That's simply the measurement error. In other tooling such
  as `sysstat` it seems to be common practice to _asymmetrically_ hide
  measurement uncertainty by capping values when they are known to in theory not
  exceed a certain threshold
  ([example](https://github.com/sysstat/sysstat/commit/52977c479d3de1cb2535f896273d518326c26722)).
