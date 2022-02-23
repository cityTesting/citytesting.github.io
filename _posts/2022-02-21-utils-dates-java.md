---
layout: post
title: Utils for dates in Java
categories: [dates, java]
---

Here are some interesting utilities for working with dates in java:

**Convert LocalDate to Date**  
{% highlight java %}
  private Date convertLocalDateTimeToDate(LocalDateTime localDateTime){
        return Date.from(localDateTime.atZone(ZoneId.systemDefault()).toInstant());
    }
{% endhighlight %}    
**Convert Date to LocalDate**  
{% highlight java %}
  private LocalDateTime convertDateToLocalDateTime(Date date){
        return date.toInstant().atZone(ZoneId.systemDefault()).toLocalDateTime();
    }
{% endhighlight %}    
