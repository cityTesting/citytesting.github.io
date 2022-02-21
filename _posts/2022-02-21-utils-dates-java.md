---
layout: post
title: Utils for dates in Java
categories: [date, java]
---
**Convert LocalDate to Date**  
```
  private Date convertLocalDateTimeToDate(LocalDateTime localDateTime){
        return Date.from(localDateTime.atZone(ZoneId.systemDefault()).toInstant());
    }
```  
**Convert LocalDate to Date**  
```
  private LocalDateTime convertDateToLocalDateTime(Date date){
        return date.toInstant().atZone(ZoneId.systemDefault()).toLocalDateTime();
    }
```