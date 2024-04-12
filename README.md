# xml-collection

## select로 조회된 결과를 컬렉션(List)에 담아지정된 필드에 세팅
			
 property : List를 담을 DTO의 필드명
	
 select : 실행할 select의 id
	
 column : 조회 결과 중 지정된 컬럼의 값을 파라미터로 전달
	
 javaType : List(컬렉션)의 타입을 지정
	
 ofType : List(컬렉션)의 제네릭(타입 제한) 지정
		
	
 mapper의 변수명과 같은 id의 쿼리가 실행되고, 
	
 그 결과는 쿼리의 id와 같은 select 속성을 가진 
	
 <collection> 태그에서 정의된 property 속성의 DTO 값에 저장된다.

## 작성예시 

###쿼리
###<select id="selectCommentList" resultType = "Comment">
 		SELECT LEVEL, C.* FROM
			(SELECT COMMENT_NO, COMMENT_CONTENT,
			    TO_CHAR(COMMENT_WRITE_DATE, 'YYYY"년" MM"월" DD"일" HH24"시" MI"분" SS"초"') COMMENT_WRITE_DATE,
			    BOARD_NO, MEMBER_NO, MEMBER_NICKNAME, PROFILE_IMG, PARENT_COMMENT_NO, COMMENT_DEL_FL
			FROM "COMMENT"
			JOIN MEMBER USING(MEMBER_NO)
			WHERE BOARD_NO = #{boardNo}) C
		WHERE COMMENT_DEL_FL = 'N'
		OR 0 != (SELECT COUNT(*) FROM "COMMENT" SUB
						WHERE SUB.PARENT_COMMENT_NO = C.COMMENT_NO
						AND COMMENT_DEL_FL = 'N')
		START WITH PARENT_COMMENT_NO IS NULL
		CONNECT BY PRIOR COMMENT_NO = PARENT_COMMENT_NO
		ORDER SIBLINGS BY COMMENT_NO
 	</select>

