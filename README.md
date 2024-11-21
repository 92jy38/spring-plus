fix: Lv1-1 @transactional 이해.
- 에러 로그를 보고 read-only로 인해 insert를 허용하지 않는 상황이 발생한 것으로 판단.
- todoService에 @transactional(readOnly = true)->(readOnly = false) 로 변경. default는 false라 지워도 되지만 명시하기 위해 false로 변경.

fix: Lv1-2 JWT의 이해
- User 테이블에 nickname 컬럼 추가.
- 요구사항에 닉네임은 중복 가능인 점으로 보아 추가 로직은 필요 없을 것으로 판단.
- "JWT에서 닉네임 꺼내 보여주길 원하는 부분"을 보고 response와 관련된 부분 수정 후, 생성자 파라미터들과 서비스단에서 필요한 부분들 수정.
- jwtuill 과 auth 관련 부분들 수정.

fix: Lv2-6 CASCADE
CascadeType.PERSIST 사용해 영속화하여 할 일을 생성한 유저가 담당자로 등록되도록 함.

fix: Lv2-7 N+1 문제
- 기존 코드에서 JOIN이 사용되어 INNER JOIN 실행할텐데 N+1문제가 발생하는 이유를 모르겠음.
- 메모리 효율적인 측면에서 EntityGraph를 사용하였음.
- fetchType. Lazy 와 Eager는 static 정보로 변경 할 수 없지만 EntityGraph 로 변경가능.
- EntityGraph는 OUTER LEFT JOIN 방식 이기 때문에 N+1 발생하지 않음.
- 1:N 컬렉션 같이 조회 해 오는경우 join 결과를 application으로 가져오고 memory에서 페이징처리 하는 Fetch Join과 차이가 있음. EntityGraph는 db에서 페이징 처리.
