# AZ DEVOPS + ACR + TRIVY (SCAN)

Greetings my fellow Technology Advocates and Specialists.

Repo with code and pipeline to  __Scan Docker Images__ in __AZURE CONTAINER REGISTRY__ with __AQUASEC TRIVY__ using __AZURE DEVOPS PIPELINES__ and deploy to __AKS__

| __REQUIREMENTS:-__ |
| --------- |

1. Azure Container Registry
2. Azure Storage Account 
3. Azure Resource Manager Service Connection
4. Docker Registry (Azure Container Registry) Service Connection
5. Dockerfile
6. Sample HTML File
7. Azure DevOps Pipeline (YAML)
8. Trivy Ignore file (.trivyignore)

| __WHAT DOES THE PIPELINE DO:-__ |
| --------- |

| # | PIPELINE TASKS | 
| --------- | --------- |
| 1. | BUILD AND PUSH THE IMAGE IN ACR |
| 2. | DOWNLOAD AND INSTALL AQUASEC TRIVY | 
| 3. | EXECUTE TRIVY SCAN AND COPY THE SCAN RESULTS IN ARTIFACTS STAGING DIRECTORY |
| 4. | PUBLISH THE ARTIFACTS |
| 5. | DOWNLOAD THE PUBLISHED ARTIFACTS |
| 6. | COPY THE AQUASEC TRIVY SCAN REPORTS TO BLOB STORAGE CONTAINER WITH DATE TIME STAMP DIRECTORY  |


| __WHY IS TRIVY IGNORE FILE (.trivyignore) REQUIRED ?__ |
| --------- |
| After Scanning of the Image, we identify LOW, MEDIUM, HIGH and CRITICAL Vulnerabilities. The CVE (Common Vulnerabilities and Exposures) gets listed in the Report. If for some reasons, Application team accepts the risk and wants to skip the LOW and MEDIUM Vulnerabilities from the Scan report, all we have to do is list the respective CVEs in the .trivyignore file and run the pipeline again to scan. The listed CVEs will no longer be in the Scan Report. |


| __Below follows the contents of the Docker File:-__ |
| --------- |
```
FROM nginx:1.15.9-alpine
COPY . /usr/share/nginx/html
```

| __Below follows the contents of the HTML File:-__ |
| --------- |

```
<html>
<head>
    <title>PoC pipe com scan - Trivy</title>
</head>

</style>
<body>
    <h1 style="font-size:50px; color:#000000; text-align: center">POC - AZ DEVOPS + TRIVY + ACR</h1>
</body>
</html>
```

| __azure-pipelines.yml is the YAML File (Azure DevOps) with the pipeline code-__ |
| --------- |

| __Below follows the contents of Trivy Ignore File (.trivyignore):-__ |
| --------- |

```
# All LOW and MEDIUM Vulnerabilities has been ignored (Except 2)!!!
#CVE-2018-5711
#CVE-2018-14048
CVE-2018-14498
CVE-2019-1547
CVE-2019-1549
CVE-2019-1551
CVE-2019-1563
CVE-2019-7317
CVE-2019-11038
CVE-2019-12904
CVE-2019-13627
CVE-2020-1971
CVE-2020-14155
CVE-2020-15999
CVE-2020-24977
CVE-2020-28928
CVE-2021-3449
CVE-2021-23841
CVE-2021-23839
```

| __PIPELINE RESULTS:-__ |
| --------- |
| ![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4x8q2ujk8ney1vtp0b2k.png) |

| __ARTIFACTS PUBLISHED:-__ |
| --------- |
| ![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gkiwsquultdc8h7403t9.png) |
| ![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/oibtfxw2k4eqyvstr02b.png) |

| __IMAGE SCAN REPORT STORED INSIDE STORAGE CONTAINER UNDER DATE TIME STAMP DIRECTORY:-__ |
| --------- |
| ![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/e2umu2vymusgdove9db7.png) |
| ![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mebah8oubi7pnw2ta0rn.png) |

| __HOW IMAGE SCAN REPORT LOOKS LIKE:-__ |
| --------- |

| HIGH AND CRITICAL VULNERABILITIES IMAGE SCAN REPORT:- |
| --------- |
| ![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/s1rr0rn6cgnx210asc39.png) |

| LOW AND MEDIUM VULNERABILITIES IMAGE SCAN REPORT:- |
| --------- |
| ![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/y83jm776l1ux8xlqj1yc.png) |
| __Note-__ The Only Reason we see 2 CVEs with Severity MEDIUM is because they are commented out in __.trivyignore__ file |


