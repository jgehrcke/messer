
## 0.3.0 (2019-09-20)

`goeffel-analysis`:

- Rename `magic` command to `plot`, and `plot` to `flexplot`.
- `plot`:
    - New default metric `proc_mem_rss_percent`, do not plot `system_loadavg1` anymore by default.
    - Cosmetic changes (improved subtitle, and others)
    - More complete help output.
    - Set rolling window width to 10 s.
    - Clean up log output a bit.


## 0.2.0 (2019-08-13)

Renamed to Goeffel.

`goeffel`:

- Consolidate sampling loop, make the sampling interval much more predictable.
- Add `monotime` metric.
- Fix bug as of which every Nth sample was not emitted to the output file.
- Consolidated error handling around a lack of privileges, and implement better exit code control.
- Add `trialprocess` gimmick.
- Command line interface cleanup, among others:
    - Add `-i` short option (for `--sampling-interval`).
    - Add `--label` option.
    - Improve help text.

`goeffel-analysis`:

- Fix a bug in the absence of `--metric`.
- Do not emit PDF files anymore by default.
- Only conditionally do a symlog plot for certain metrics.
- Tweak layout of plot elements.
- Enhance compatibility with output files emitted by previous versions.
- Make `plot` command non-interactive.
- `magic` command: add support for `proc_num_ip_sockets` metric.


## 0.1.0 (2019-07-31)

Initial public release. Was used and developed internally prior to this.