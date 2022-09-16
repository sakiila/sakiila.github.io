---
title: GrayUtils 灰度配置工具类
tags:
- Java
- Gray 
categories: 开发
description: <center>使用 GrayUtils 灰度配置工具类来控制灰度开关和流量。</center>
abbrlink: 2646382575
date: 2022-01-20 21:15:00
---
# 一、使用方式

1. 添加相关配置，参考配置示例；
2. 使用 `GrayUtils.IsHitGray(MCC配置, ld)` 方法，进行灰度判断。

# 二、配置示例

```json
{
 "needGray": true,
 "needCheck": true,
 "whiteldList":［1386999，13870001],
 "whitePercent": "64.5"
}
```

参数解释：

- needGray：灰度总开关，Boolean 类型，为 true 则继续灰度判断流程，为 false 则跳过判断执行原有逻辑。
- needCheck：校验开关，Boolean 类型。为 true 则继续灰度判断流程，为 false 则跳过判断执行灰度逻辑（等于全量灰度）。
- whiteldList：白名单配置列表，Long 类型数组，命中则执行灰度逻辑，未命中或为空则继续灰度判断流程。
- whitePercent：百分比配置字符串，数字格字符串，可以为小数（最好不超过一位），命中则执行灰度逻辑，未命中或为空则执行原有逻辑。举例：“64.5“ 表示 64.5％ 以下的请求执行灰度逻辑。

# 三、执行流程

![GrayUtils](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/28ff9937-b3d6-4548-a3d2-33a61cddc439/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220916T110340Z&X-Amz-Expires=86400&X-Amz-Signature=502f9e912ba4400dfc93b7b86f5ca93aacb17484672d20dad7b6fd3769564855&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

# 四、示例代码

```java
import com.alibaba.fastjson.JSONObject;
import lombok.Data;
import org.apache.commons.collections.CollectionUtils;
import org.apache.commons.lang3.StringUtils;

import java.math.BigDecimal;
import java.util.List;

/**
 * 灰度工具
 * @see <a href="https://km.sankuai.com/page/1204474051">使用教程</a>
 */
public class GrayUtils {
    
    public static boolean isHitGray(String jsonString) {
        GrayConfDTO grayConfDTO = JSONObject.parseObject(jsonString, GrayConfDTO.class);
        return grayConfDTO.isHitGray();
    }
    
    public static boolean isHitGray(String jsonString, Long id) {
        GrayConfDTO grayConfDTO = JSONObject.parseObject(jsonString, GrayConfDTO.class);
        return grayConfDTO.isHitGray(id);
    }
    
    public static boolean isHitGrayWithoutId(String jsonString) {
        GrayConfDTO grayConfDTO = JSONObject.parseObject(jsonString, GrayConfDTO.class);
        return grayConfDTO.isHitGrayWithoutId();
    }

    @Data
    static class GrayConfDTO {
        /**
         * 灰度总开关
         */
        private Boolean needGray;

        /**
         * 灰度校验开关
         */
        private Boolean needCheck;

        /**
         * 灰度列表
         */
        private List<Long> whiteIdList;

        /**
         * 灰度百分比
         */
        private String whitePercent;

        public boolean isHitGray() {
            return needGray;
        }

        public boolean isHitGray(Long id) {
            // 总开关关闭，执行原有逻辑
            if (!needGray) {
                return Boolean.FALSE;
            }

            // 不需要校验，执行新逻辑
            if (!needCheck) {
                return Boolean.TRUE;
            }

            // id 为空，执行原有逻辑
            if (id == null) {
                return  Boolean.FALSE;
            }

            // id 在列表中，执行新逻辑
            if (CollectionUtils.isNotEmpty(whiteIdList)
                    && whiteIdList.contains(id)) {
                return Boolean.TRUE;
            }

            // id 在百分比中，执行新逻辑
            if (StringUtils.isNotBlank(whitePercent)) {
                return isHitPercent(id, whitePercent);
            }

            return Boolean.FALSE;
        }

        public boolean isHitGrayWithoutId() {
            // 总开关关闭，执行原有逻辑
            if (!needGray) {
                return Boolean.FALSE;
            }

            // 不需要校验，执行新逻辑
            if (!needCheck) {
                return Boolean.TRUE;
            }

            // 根据时间戳进行灰度（暂行）
            if (StringUtils.isNotBlank(whitePercent)) {
                return isHitPercent(System.currentTimeMillis(), whitePercent);
            }

            return Boolean.FALSE;
        }

        /**
         * 判断是否命中百分比配置
         *
         * @param id
         * @param whitePercent
         * @return
         */
        private Boolean isHitPercent(Long id, String whitePercent) {
            // 被除数
            BigDecimal dividend = new BigDecimal(id);
            // 除数
            BigDecimal divisor = new BigDecimal("100");
            // 余数
            BigDecimal remainder = new BigDecimal(whitePercent.trim());

            // 计算小数点后位数 n
            int decimalPlaces = countDecimalPlaces(whitePercent);
            // 放大 10 ^ n
            divisor = divisor.scaleByPowerOfTen(decimalPlaces);
            remainder = remainder.scaleByPowerOfTen(decimalPlaces);

            // 取余
            BigDecimal idRemainder = dividend.remainder(divisor);

            // 比较
            if (idRemainder.compareTo(remainder) < 0) {
                return Boolean.TRUE;
            }
            return Boolean.FALSE;
        }

        /**
         * 计算小数点后位数
         * 12 = 0
         * 12.5 = 1
         * 12.50 = 2
         *
         * @param stringNumber
         * @return
         */
        private int countDecimalPlaces(String stringNumber) {
            if (!stringNumber.contains(".")) {
                return 0;
            }
            return stringNumber.length() - stringNumber.indexOf('.') - 1;
        }
    }
}
```