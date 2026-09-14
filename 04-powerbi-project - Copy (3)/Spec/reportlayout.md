Page 1

KPI 1 Card Mapping
| Visual Element | Measure                        |
| -------------- | ------------------------------ |
| Main Value     | Historical Fostered Animals    |
| Start Year     | First Reporting Year           |
| End Year       | Latest Reporting Year          |
| Subtitle       | Historical Participation Label |


KPI 2 Card Mapping
| Visual Element | Measure                                |
| -------------- | -------------------------------------- |
| Main Value     | Program Growth                         |
| Start Year     | First Reporting Year                   |
| Start Value    | Baseline Fostered Animals              |
| End Year       | Latest Completed Year                  |
| End Value      | Latest Completed Year Fostered Animals |


| Visual Element       | Measure                                | DAX Purpose                                                                                      | Display Folder | Description                                                                          |
| -------------------- | -------------------------------------- | ------------------------------------------------------------------------------------------------ | -------------- | ------------------------------------------------------------------------------------ |
| Main Value           | Latest Month Fostered Animals          | Latest month Fostered Animals using latest `Month Date Key` from Foster Animal Month fact table. | Benchmarking   | Distinct animals that participated in foster care during the latest reporting month. |
| Previous Year Value  | Prior Year Same Month Fostered Animals | Same month prior year Fostered Animals count.                                                    | Benchmarking   | Used for year-over-year monthly comparison.                                          |
| YoY Variance %       | Prior Year Variance %                  | `(Current Month - Prior Year Same Month) / Prior Year Same Month`                                | Benchmarking   | Month-over-month year-over-year performance change.                                  |
| Comparison Line      | Prior Year Comparison Label            | Formats result as `▲ 2.3% vs August 2025` or `▼ 0.04% vs August 2025`.                           | Benchmarking   | Display label shown directly below KPI value.                                        |
| Benchmark Value      | 5-Year Monthly Benchmark               | Average same-month Fostered Animals across previous 5 completed years.                           | Benchmarking   | Historical seasonal benchmark for the latest reporting month.                        |
| Benchmark Variance % | Monthly Benchmark Variance %           | `(Current Month - Benchmark) / Benchmark`                                                        | Benchmarking   | Performance against historical seasonal benchmark.                                   |
| Benchmark Label      | 5-Year Benchmark Label                 | Formats benchmark period as `2021-2025 Avg`.                                                     | Benchmarking   | Shows benchmark period used in calculation.                                          |
| Status Text          | Monthly Performance Status             | Returns `🟢 On Target`, `🟡 Monitor`, or `🔴 Action Required`.                                   | Benchmarking   | KPI status based on benchmark variance thresholds.                                   |
| Status Color         | Monthly Performance Status Color       | Returns hex color (`#4CAF50`, `#FFC107`, `#F44336`).                                             | Benchmarking   | Conditional formatting color for KPI status.                                         |



KPI 3 Card Mapping
| Visual Element       | Measure                         | DAX Definition                                                                                        | Display Folder   | Description                                                                                                             |
| -------------------- | ------------------------------- | ----------------------------------------------------------------------------------------------------- | ---------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Main Value           | YTD Fostered Animals            | Current year Fostered Animals from January through latest reporting month.                            | YTD Benchmarking | Distinct animals that participated in foster care during the current reporting year through the latest reporting month. |
| Previous Year Value  | Prior Year YTD Fostered Animals | Previous year Fostered Animals from January through same reporting month.                             | YTD Benchmarking | Used for YTD year-over-year comparison.                                                                                 |
| YoY Variance %       | Prior Year YTD Variance %       | `(YTD Fostered Animals - Prior Year YTD Fostered Animals) / Prior Year YTD Fostered Animals`          | YTD Benchmarking | Percentage change compared with prior year YTD.                                                                         |
| Comparison Line      | Prior Year YTD Comparison Label | Format variance measure into text such as `▼ 0.5% vs Aug 2025 YTD`.                                   | YTD Benchmarking | Supporting KPI context label.                                                                                           |
| Benchmark Value      | 5-Year YTD Benchmark            | Average YTD Fostered Animals across previous 5 completed years using the same reporting month cutoff. | YTD Benchmarking | Historical benchmark used for YTD performance evaluation.                                                               |
| Benchmark Variance % | YTD Benchmark Variance %        | `(YTD Fostered Animals - 5-Year YTD Benchmark) / 5-Year YTD Benchmark`                                | YTD Benchmarking | Percent variance versus historical benchmark.                                                                           |
| Benchmark Label      | 5-Year YTD Benchmark Label      | Format benchmark period into text such as `2021-2025 Avg`.                                            | YTD Benchmarking | Shows benchmark years used.                                                                                             |
| Status Text          | YTD Performance Status          | Evaluate `ABS(YTD Benchmark Variance %)` using executive threshold rules.                             | YTD Benchmarking | Executive performance interpretation.                                                                                   |
| Status Color         | YTD Performance Status Color    | Returns benchmark KPI color (`#4CAF50`, `#FFC107`, `#F44336`).                                        | YTD Benchmarking | Used for conditional formatting.                                                                                        |


KPI 5 
Historical Completion Ratio
YTD Fostered Animals
Prior Year Actual Fostered Animals
Forecast Variance %
Forecast Comparison Label
Forecast Status
Forecast Status Color
