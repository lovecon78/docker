```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="your.namespace.yourMapper">

    <!-- select${테이블명}List -->
    <select id="select${테이블명}List" parameterType="map" resultType="egovMap">
        /* your.namespace.yourMapper.select${테이블명}List */
        SELECT 
            ${columnList}
        FROM 
            ${tableName}
        WHERE 
            DEL_YN = 'N'
        <if test="searchCondition != null and searchCondition != ''">
            AND ${searchCondition} LIKE CONCAT('%', #{searchValue}, '%')
        </if>
        ORDER BY 
            ${sortColumn} ${sortOrder}
    </select>

    <!-- select${테이블명}Detail -->
    <select id="select${테이블명}Detail" parameterType="map" resultType="egovMap">
        /* your.namespace.yourMapper.select${테이블명}Detail */
        SELECT 
            ${columnList}
        FROM 
            ${tableName}
        WHERE 
            ${primaryKey} = #{${primaryKeyColumn}}
    </select>

    <!-- insertUpdate${테이블명} -->
    <insert id="insertUpdate${테이블명}" parameterType="map">
        /* your.namespace.yourMapper.insertUpdate${테이블명} */
        MERGE INTO ${tableName} T
        USING (SELECT 
                   #{${column1}} AS ${column1},
                   #{${column2}} AS ${column2},
                   -- 추가 컬럼들...
                   #{${primaryKeyColumn}} AS ${primaryKeyColumn}
               FROM DUAL) S
        ON (T.${primaryKeyColumn} = S.${primaryKeyColumn})
        WHEN MATCHED THEN
            UPDATE SET
                ${column1} = S.${column1},
                ${column2} = S.${column2},
                -- 추가 업데이트 컬럼들...
                UPDATE_DT = SYSDATE
        WHEN NOT MATCHED THEN
            INSERT (
                ${column1},
                ${column2},
                -- 추가 삽입 컬럼들...
                ${primaryKeyColumn},
                CREATE_DT,
                UPDATE_DT
            ) VALUES (
                S.${column1},
                S.${column2},
                -- 추가 삽입 컬럼들...
                S.${primaryKeyColumn},
                SYSDATE,
                SYSDATE
            )
    </insert>

    <!-- delete${테이블명} -->
    <update id="delete${테이블명}" parameterType="map">
        /* your.namespace.yourMapper.delete${테이블명} */
        UPDATE ${tableName}
        SET 
            DEL_YN = 'Y',
            UPDATE_DT = SYSDATE
        WHERE 
            ${primaryKeyColumn} = #{${primaryKeyColumn}}
            AND DEL_YN = 'N'
    </update>

</mapper>
```