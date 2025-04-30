# Granular Anonymization Policies

This document describes the case where a blanket policy of redacting or anonymizing the entire contents of a data field may hamper detectors used to identify cyber threats.  With filepaths, the non-PII portions of a filepath may be significant for determining if a system has been compromised, such as executables started under the user's `startup` directory.  Similarly, a system may include the username in the URL for a GET request to internal services, but the domain and other encoded arguments are indicative of a compromised system.

If feasible, supporting selective anonymization for common data fields, such as filepaths and URLs would be ideal.  However, given varying requirements from the diversity of possible end users, giving them the ability to apply the anonymizer's strategies to just a subset of of certain data fields may be desirable.  We note that this may be risky, so facilities to warn end users of potential errors and information spills from any custom coded schemes will be helpful.  

We folllow with an example that illustrate where PII can be present in certain fields, and how certain portions of that field can be important for detecting threats.  We conclude with a note on any end-user directed or coded granular anonymization policies.

## Example, Windows Filepaths

A popular attack vector is to place malicious shortcuts and executables into the users startup directory.  On Windows, the user's startup directory follows this template:
  
```
  C:\Users\%username%\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
```

Where `%username%` corresponds to the username.  For example, user `BillieG` has a shortcut (LNK file) in their Startup directory that automatically launches a pomodoro timer application on every login:

```
  C:\Users\BillieG\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\pomodoro.lnk
```

This automatic launch on boot mechanism is also [leveraged by malware](https://www.cybereason.com/hubfs/Insights/Research/threat-analysis-purple-team-taking-shortcuts-LNK-files.pdf) to run executables or perform system calls that allow them to maintain persistence on a compromised Windows system.  

Standard anonymization strategies would anonymize or ablate the entire field where PII is identified.  While more secure, this has the unfortunate side effect of removing the ability to identify which items are slated to automatically launch on startup.  To preserve these details, we recommend a capability (implemented either by the system or by end users) that allows only portions of the filepath that contain the user name be anonymized, e.g., the anonymizer maps `BillieG` to `USER123`, 

```
  C:\Users\USER123\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\pomodoro.lnk
```

Note, being able to anonymize or redact where usernames will occur in filepaths is important, as there may be corner cases for direct anonymization schemes.  For example, a blanket regular expression search a userid based on the surname `Sys` (rare, but [it does exist](https://en.geneanet.org/surnames/SYS)) would impact filepaths that can be informative of a system compromise, such as execution or file writes to paths beginning with `C:\Windows\System`.

## User-directed Anonymization Policies

A natural avenue would be to offer end users the ability to select which subset of a field be subject to anonymization.  For example, an organization may choose to break apart a filepath and pass each component directory or filename to the system for anonymization.  While this may be the best way to retain the semantics of the field after anonymization, there is the real risk the end users' code or policies may be buggy and inadvertantly spill PII.  If this route is taken, we recommend warning and debugging capabilities be put into place to mitigate this.