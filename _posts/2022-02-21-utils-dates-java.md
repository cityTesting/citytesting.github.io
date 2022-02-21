---
layout: post
title: Utils for dates in Java
categories: [dates, java]
---

Here are some interesting utilities for working with dates in java:

**Convert LocalDate to Date**  
```
  private Date convertLocalDateTimeToDate(LocalDateTime localDateTime){
        return Date.from(localDateTime.atZone(ZoneId.systemDefault()).toInstant());
    }
```    
**Convert Date to LocalDate**  
```
  private LocalDateTime convertDateToLocalDateTime(Date date){
        return date.toInstant().atZone(ZoneId.systemDefault()).toLocalDateTime();
    }
```