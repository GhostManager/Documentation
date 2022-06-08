---
description: Description and explanation of common API actions
---

# Common API Actions



```
mutation CheckoutDomain {
  checkoutDomain(activityTypeId: 1, domainId: 1, endDate: "2022-5-22", projectId: 1, startDate: "2022-5-18") {
    result
  }
}
```



```
mutation GenerateReport {
  generateReport(id: 1) {
    docxUrl
    pptxUrl
    xlsxUrl
    reportData
  }
}
```
