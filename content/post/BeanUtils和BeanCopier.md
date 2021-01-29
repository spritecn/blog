---
title: "BeanCopier、BeanUtils对象属性拷贝"
date: 2021-01-12T19:31:42+08:00
description: ""
tags: [ "hugo","blog" ]
categories: [ "shitLife" ]
weight: 40
draft: false
---

# BeanCopier、BeanUtils对象属性拷贝

```java
import org.springframework.beans.BeanUtils;
import org.springframework.cglib.beans.BeanCopier;
import spock.lang.Specification;
class test extends Specification {
    def "test BeanUtils.copyProperties"(){
        given:
            FormatDTO formatDTO = new FormatDTO();
            formatDTO.setStartTime(new Date())
            formatDTO.setCostPrice(BigDecimal.ONE)
            CdrDTO cdrDTO = new CdrDTO()
            CdrDTO cdrDTO2 = new CdrDTO()
        when:
            BeanUtils.copyProperties(formatDTO,cdrDTO)
            BeanCopier.create(YmyCallrecordFormatDTO,SaveCallrecordYmyDTO,false).copy(formatDTO,cdrDTO2,null)
        then:
            formatDTO.getStartTime() == cdrDTO.startTime
            formatDTO.costPrice == cdrDTO.costPrice
            formatDTO.getStartTime() == cdrDTO2.startTime
            formatDTO.costPrice == cdrDTO2.costPrice
        
    }
}
```
经测试，均可以正常处理 BigDecimal和java.util.Date