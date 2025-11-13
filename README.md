# gpudash

This fork uses `sinfo` to get list of nodes and queries Prometheus with specified period and interval to get GPU utilization data and display it in a text dashboard. By default it shows GPU utilization for all users over the last hour with 10-minute intervals.

The original implementation requires a static list of nodes to be specified in the code. As well as cronjob to capture GPU utilization from prometheus and store in text files, I found that counter-intuitive, because prometheus is already storing that data.

---

The `gpudash` command displays a GPU utilization dashboard in text (no graphics) for the last hour:

![gpudash example](images/gpudash.png)

The dashboard can be generated for a specific user:

![gpudash user example](images/gpudash_user.png)

The `gpudash` command is part of the [Jobstats platform](https://github.com/PrincetonUniversity/jobstats). Here is the help menu:

```
usage: gpudash.py [-h] [-u NETID] [-n] [--me] [-c CHARS_PER_CELL] [-p PERIOD] [-i INTERVAL]

GPU utilization dashboard

options:
  -h, --help            show this help message and exit
  -u NETID              create dashboard for a single user
  -n, --no-legend       flag to hide the legend
  --me                  create dashboard for current user
  -p PERIOD, --period PERIOD
                        time period to show (e.g., 1h, 2h, 30m). Default: 1h
  -i INTERVAL, --interval INTERVAL
                        sampling interval (e.g., 5m, 10m, 15m). Default: 10m

Utilization is the percentage of time during a sampling window (< 1 second) that
a kernel was running on the GPU. The format of each entry in the dashboard is
username:utilization (e.g., aturing:90). Utilization varies between 0 and 100%.

Examples:

  Show dashboard for all users (last hour):
    $ gpudash

  Show dashboard for last 2 hours with 10-minute intervals:
    $ gpudash --period 2h --interval 10m

  Show dashboard for the current user:
    $ gpudash --me

  Show dashboard for the user aturing:
    $ gpudash -u aturing

  Show dashboard for all users without displaying legend:
    $ gpudash -n
```

## Getting Started

`gpudash` is a pure Python code. Its only dependency is the `blessed` Python package. On Debian Linux, this can be installed with:

```bash
apt-get install python3-blessed
```

Then put `gpudash` in a location like `/usr/local/bin`:

```
cd /usr/local/bin
wget https://raw.githubusercontent.com/astappiev/gpudash/main/gpudash
chmod 755 gpudash
```

Next, edit `gpudash` by replacing `PROM_URL` with a url to Prometheus instance. Alternativelly, you can set the `GPUDASH_PROM_URL` environment variable to the Prometheus url.

With these steps in place, you can use the `gpudash` command:

```
gpudash
```

## Getting Help

Please post an issue to this repo. Extensions to the code are welcome via pull requests.
