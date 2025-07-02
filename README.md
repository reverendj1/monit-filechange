# monit-filechange

monit-filechange - monit-filechange is a script to check if a file has changed size since monit-filechange
was last run. Although it's designed to work with a monitoring system such as m/monit, and as such, has proper exit
codes, it can be used by itself too. This is handy for using in conjunction with scripts.

## Usage

```bash
monit-filechange [OPTIONS] [FILE]
```

## Arguments

| argument                 | description |
| ------------------------ | ----------- |
| -h, --help               | Prints this help text |
| -C, --change             | Errors if the file has changed size at all |
| -c, --same               | Errors if the file has stayed the same size |
| -S, --grow               | Errors if the file has grown at all |
| -s, --shrink             | Errors if the file has shrunk at all |
| -P, --growper=PERCENT    | Errors if the file has grown by PERCENT percent |
| -p, --shrinkper=PERCENT  | Errors if the file has shrunk by PERCENT percent |
| -B, --growb=SIZE         | Errors if the file has grown by SIZE b |
| -b, --shrinkb=SIZE       | Errors if the file has shrunk by SIZE b |
| -K, --growkb=SIZE        | Errors if the file has grown by SIZE kb |
| -k, --shrinkkb=SIZE      | Errors if the file has shrunk by SIZE kb |
| -M, --growmb=SIZE        | Errors if the file has grown by SIZE mb |
| -m, --shrinkmb=SIZE      | Errors if the file has shrunk by SIZE mb |
| -G, --growgb=SIZE        | Errors if the file has grown by SIZE gb |
| -g, --shrinkgb=SIZE      | Errors if the file has shrunk by SIZE gb |

## Examples

```bash
# error if file has changed size since monit-filechange was last ran
monit-filechange /etc/someapp/app.conf

# error if file has grown by 10% since monit-filechange was last ran
monit-filechange --growper 10 /var/log/somelog.log

# error if file has shrunk by 100MB since monit-filechange was last ran
monit-filechange --shrinkmb 100 /var/log/somelog.log

# error if file has shrunk at all since monit-filechange was last ran
monit-filechange --shrink /var/lib/somedb/mydb.db
```

## Monit Example

```text
check program log_shrink with path "/usr/local/bin/monit-filechange --shrinkmb 100 /var/log/somelog.log"
  if status != 0 then alert
```

## Exit Codes

0: no errors
1: file has changed size when it shouldn't
2: file has not changed size when it should
3: file has grown when it shouldn't
4: file has shrunk when it shouldn't
5: syntax error
6: other error

## Requirements

* bash

Creates a persistent state file, /var/lib/monit-filechange.ini to store file sizes
