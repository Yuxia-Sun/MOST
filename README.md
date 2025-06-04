# MOST: a dataset for Malware Open-Set  classification with Time-consistent splitting

## Introduction

This is a novel PE malware dataset which comprises 34,343 malware samples across 62 families. The malware samples are assigned reliable family labels and annotated with first-seen timestamps based on [VirusTotal](https://www.virustotal.com/gui/home/upload) reports, covering a span of over 10 years from October 2008 to January 2019. 

<!-- 
### Data preprocessing

To construct the dataset, we first collected more than 50,000 PE malware files from [VirusSign](https://www.virussign.com/) and removed the packed files. We then utilized [EUPHONY](https://github.com/fmind/euphony) and [AVClass](https://github.com/malicialab/avclass) alongside VirusTotal reports to process the dataset. Malware files with zero, inconsistent, incomplete, or ambiguous tags were removed, and the remaining files were assigned their respective malware family labels. Next, we obtained the earliest time of the malware files submitted to the VirusTotal and timestamped the malware samples with that time. After that, we utilized [Ember](https://github.com/elastic/ember) to extract **raw characteristics from malware binaries**, applied [KaggleSPbun](https://github.com/SPbun/malware-detection) to extract **instruction-level characteristics from malware disassembly files**, and combined them to form a 2,807-dimensional vector for each malware sample. Finally, we obtained 34,343 malware sample vectors spanning 62 families and resized each vector into a **54×54 malware characteristic image.**
-->

### Time-consistent splitting

We split MOST into training and test sets using **December 5, 2018** as the split point. In addition, we varied the number of large families containing over 100 malware samples in the training set and obtained different settings.

To make sure that we have sufficient malware samples in the training set to avoid overfitting, we selected the top M largest families as seen families, where M = 10, 20, or 30, as shown in the following table:

<table><thead>
  <tr>
    <th rowspan="2">#Seen : #Unseen</th>
    <th></th>
    <th colspan="2">Seen Families</th>
    <th colspan="2">Unseen Families</th>
  </tr>
  <tr>
    <th></th>
    <th>#Sample</th>
    <th>#Family</th>
    <th>#Sample</th>
    <th>#Family</th>
  </tr></thead>
<tbody>
  <tr>
    <td rowspan="2">10:52</td>
    <td>Training set</td>
    <td>18,970</td>
    <td>10</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>Test set</td>
    <td>4,654</td>
    <td>10</td>
    <td>2,316</td>
    <td>52</td>
  </tr>
  <tr>
    <td rowspan="2">20:42</td>
    <td>Training set</td>
    <td>22,816</td>
    <td>20</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>Test set</td>
    <td>5,443</td>
    <td>20</td>
    <td>1,527</td>
    <td>42</td>
  </tr>
  <tr>
    <td rowspan="2">30:32</td>
    <td>Training set</td>
    <td>25,037</td>
    <td>30</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>Test set</td>
    <td>5,838</td>
    <td>30</td>
    <td>1,132</td>
    <td>32</td>
  </tr>
</tbody></table>

For example, in the first setting, the training set contains 18,970 samples from 10 seen families, while the test set has 4,654 samples from 10 seen families and 2,316 samples from 52 unseen families. In all settings, **the training set includes samples found before the split point, and the test set includes those found afterward.**

## Usage

This repository is organized as follows:

```
/
│  
└─data
    ├─10-52
    │  ├─training
    │  │  ├─0
    │  │  ├─1
    │  │  ├─...
    │  │  └─9
    │  └─testing
    │     ├─0
    │     ├─1
    │     ├─...
    │     └─61
    │  
    ├─20-42
    │  ├─training
    │  │  ├─0
    │  │  ├─1
    │  │  ├─...
    │  │  └─19
    │  └─testing
    │     ├─0
    │     ├─1
    │     ├─...
    │     └─61
    │   
    └─30-32
       ├─training
       │  ├─0
       │  ├─1
       │  ├─...
       │  └─29
       └─testing
          ├─0
          ├─1
          ├─...
          └─61
```

The `data/` directory contains datasets for three distinct experimental settings. Each setting is represented by a subdirectory named in the format `XX-YY`, where `XX` signifies the number of seen families and `YY` signifies the number of unseen families. For example, the `10-52` directory corresponds to a setting with 10 seen families and 52 unseen families.

Within each setting's directory (e.g., `data/10-52/`):

- The `training` subdirectory exclusively contains samples from the **seen families**. These are further organized into numerically named subdirectories. For instance, in `data/10-52/training/`, subdirectories are named `0` through `9`, each representing one of the 10 seen families.
- The `testing` subdirectory contains samples from **both seen and unseen families**. These are also organized into numerically named subdirectories. For example, in `data/10-52/testing/`, subdirectories are named `0` through `61`, encompassing all 10 seen families and 52 unseen families.

Finally, each numerically named subdirectory (e.g., `data/10-52/training/0/` or `data/10-52/testing/61/`) holds the actual data samples belonging to that specific family. Each sample is a 54×54 static feature image.
