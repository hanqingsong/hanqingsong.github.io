---
title: java中long型string型ip地址转换
categories:
  - java
tags:
  - string
date: 2018-05-17 09:52:23
---


```
public class IPConvertor {

    /**
     * long转ip地址，大端（高位字节放在低地址端）
     * @param ip
     * @return
     */
    public static String numToIPBigEndian(long ip) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i <= 3; i++) {
            sb.append((ip >>> (i * 8)) & 0x000000ff);
            if (i != 3) {
                sb.append('.');
            }
        }
        return sb.toString();
    }

    /**
     * long转ip地址，小端（低位字节放在低地址端）
     * @param ip
     * @return
     */
    public static String numToIPLittleEndian(long ip) {
        StringBuilder sb = new StringBuilder();
        for (int i = 3; i >= 0; i--) {
            sb.append((ip >>> (i * 8)) & 0x000000ff);
            if (i != 0) {
                sb.append('.');
            }
        }
        return sb.toString();
    }


    public static long ipToNumLittle(String ip) {
        long num = 0;
        String[] sections = ip.split("\\.");
        int i = 3;
        for (String str : sections) {
            num += (Long.parseLong(str) << (i * 8));
            i--;
        }
        return num;
    }

    // public static void main(String[] args) {
    //     long ip =571108042;
    //     String numToIPBigEndian = numToIPBigEndian(ip);
    //     System.out.println(numToIPBigEndian);
    //     System.out.println(numToIPBigEndian(1338832698));
    //     System.out.println(numToIPBigEndian(1624045370));
    //     System.out.println(numToIPBigEndian(1725232954));
    // }
}
```