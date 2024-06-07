# Using Hayabusa to Analysis Windows Threats

In this walkthrough, we'll use Hayabusa to analyze Windows 10 event log records after conducting a password spray attack using Invoke-LocalPasswordSpray. The goal is to identify compromised accounts by reviewing Hayabusa's output in Timeline Explorer.

## Prerequisites:

- SMB Sessions.
- Running **`Windows 10 VM`**.
- Hayabusa and Timeline Explorer installed in **`C:\Tools`**.

## Step-by-Step Guide:

1. Open an Administrator PowerShell Session

   1. Click Start, search for Windows PowerShell.
   2. Right-click on Windows PowerShell, select **`More > Run as administrator`**.
  
2. Navigate to the Hayabusa Directory

   ```
   cd \Tools\hayabusa
   ```

3. Examine Hayabusa Options

   Run the main Hayabusa executable to see usage and command information.

   ```
   .\hayabusa.exe
   ```

4. Generate a CSV Timeline of Threats

   Use Hayabusa to evaluate local Windows event logs and generate a CSV file with the results.

   ```
   .\hayabusa.exe csv-timeline -l -o win10-threatdetect.csv --no-color
   ```
5. Review Hayabusa Output

   The output will provide a summary of detections, including the number and severity of events. Key information to look for:

   - Total unique detections: Overall number of detection events.
   - High, medium, low, informational detections: Breakdown of detections by severity.

6. Display CSV Results

   Use PowerShell to quickly view the content of the generated CSV file.

   ```
   Get-Content .\win10-threatdetect.csv
   ```

7. Open Hayabusa CSV in Timeline Explorer

   1. Navigate to **`C:\Tools\TimelineExplorer`**.
   2. Launch **`TimelineExplorer.exe`**.
   3. Open the CSV file from **`C:\Tools\Hayabusa\win10-threatdetect.csv`**.

## Analysis in Timeline Explorer:

1. Grouping by Severity Level

    - In Timeline Explorer, drag the **`Level`** column header to the grouping area to organize alerts by severity.
  
2. Identifying Compromised Accounts

    - Focus on alerts related to logon failures, password spray, and other suspicious activities.
    - Examine high and medium severity alerts for critical events like log clears or potential malicious activities.

## Key Commands Breakdown:

- **`.\hayabusa.exe csv-timeline`**: Runs Hayabusa to generate a CSV timeline.
- **`-l`**: Reads local Windows event logs.
- **`-o win10-threatdetect.csv`**: Specifies the output file name.
- **`--no-color`**: Disables color output for better readability in PowerShell.

## Example Outputs:

### Summary of Detections:

```
Total | Unique detections: 3,110 | 35
Total | Unique critical detections: 0 (0.00%) | 0 (0.00%)
Total | Unique high detections: 3 (0.10%) | 2 (5.71%)
Total | Unique medium detections: 66 (2.12%) | 6 (17.14%)
Total | Unique low detections: 1,574 (50.61%) | 6 (17.14%)
Total | Unique informational detections: 1,467 (47.17%) | 21 (60.00%)
```

### CSV Sample:

```
"Timestamp","Computer","Channel","EventID","Level","RecordID","RuleTitle","Details"
"2023-06-12 10:18:28.878 +00:00","WindowsUser","Sec",1102,"high",34464,"Log Cleared","SubjectDomainName: WINDOWSUSER A▌ SubjectLogonId: 0x18416 A▌ SubjectUserName: Windows A▌ SubjectUserSid: S-1-5-21-2977773840-2930198165-1551093962-1000"
"2023-06-12 10:18:28.924 +00:00","WindowsUser","Sys",104,"high",19243,"Important Log File Cleared","Log: System A▌ User: Windows"
...
```

## Conclusion:

By following this walkthrough, you can effectively use Hayabusa to analyze Windows event logs and identify potential security threats. Use Timeline Explorer to further investigate and organize the results, helping you pinpoint compromised accounts and understand the scope of detected activities.

## References:

- Hayabusa GitHub: [Yamato Security](https://github.com/Yamato-Security/hayabusa)
- Timeline Explorer: [Zimmerman's Tools](https://ericzimmerman.github.io/#!index.md)
