---
title: Mybatis：映射文件的SQL深入
author: Kol Huang
date: 2020-09-04 19:22:00 +0800
categories: [Blogging, javaweb]
tags: [web]
comments: true
math: true
---



## mybatis中的动态sql语句



### if标签

```xml
<select id="findUserByCondition" parameterType="user" resultType="user">
--         在sql语句里的内容不区分大小写，但是如果不属于sql语句的内容，就需要找实体类的属性名
        select * from user where 1=1
        <if test="username != null">
            and username = #{username}
        </if>
  			<if test="userSex != null">
          	and sex = #{userSex}
  			</if>
    </select>
```



### where标签

```xml
<select id="findUserByCondition" parameterType="user" resultType="user">
--         在sql语句里的内容不区分大小写，但是如果不属于sql语句的内容，就需要找实体类的属性名
        select * from user
  			<where>
          <if test="username != null">
            	and username = #{username}
          </if>
          <if test="userSex != null">
              and sex = #{userSex}
          </if>
  			</where>
        
    </select>
```



### foreach标签

```xml
<select id="findUserInIds" resultMap="userMap" parameterType="queryvo">
        select * from user
        <where>
            <if test="ids != null and ids.size()>0">
                <foreach collection="ids" open="and id in (" close=")" item="uid" separator=",">
                    #{uid}
                </foreach>
            </if>
        </where>
    </select>
```



### sql标签

抽取重复的sql语句：

```xml
<sql id="defaultUser">
  select * from user
</sql>
  
<select ...>
  <!--可以省略不写select * from user-->
  <include refid="defaultUser"></include>
  ...
</select>
```




