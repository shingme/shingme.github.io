---
title: Map 컬렉션 반복문(HashMap, LinkedHashMap)_keySet()이용
layout: post
---

Map 컬렉션 반복문(HashMap, LinkedHashMap)_keySet()이용

Map은 기본 key,value로 이루어진 자료형이다.
key는 중복X, value는 중복O 이고, key값으로 Map에 저장된 value를 찾을 수 있다. (HashMap은 순서보장X)
Map은 인터페이스이기 때문에 이를 가져다 사용할 때 반드시 HashMap이나 LinkedHashMap으로 객체를 생성해야한다.

Map<String, String> map1 = new HashMap<String, String>();
Map<String, String> map2 = new LinkedHashMap<String, String>();


Map을 이용해 데이터를 저장했고, 이를 반복해야할 경우가 생기면 iterator()등도 가능하지만 가장 간단하게 Set를 사용할 수 있다.
내가 알기론, Map의 key들은 Set 자료형와 유사한걸로 알고 있다. Set 또한 키값으로 순서보장이 안되고 중복을 허용하지 않기 때문이다.
그래서 Map에서 Key를 keySet()메소드를 이용해 가져올 수 있다.

Set<String> set = map1.keySet();
for-eash문을 사용해 반복할 수 있다.

for(String val : set){
  map1.get(val)   //value값 찾을 수 있음
}

한번에 줄여서 
for(String val : map1.keySet()) 으로 사용가능함.



**HashMap 반복문 남긴 이유는 ibatis에서 동적으로 쿼리를 생성하는데 해당 정보를 쿼리로 넘겨줄 때 LinkedHashMap<String, ?> 이런식으로 넘겨주고 쿼리 조회 결과 데이터도 LinkedHashMap으로 받아오기 때문에 반복문이 필요해서 사용했음.
단순히 HashMap을 사용하지 않은 이유는 동적으로 select절, from절, where절을 넘겨주는데 순서가 보장되어야 되긴 때문이라고 한다. 쿼리 조회 결과도 select절에 보냈던 컬럼 순서와 맞아야되므로 순서가 중요하기 때문이라고 한다.

ex)
<select id = "getList" parameterClass="VO" resultClass="java.util.LinkedHashMap">
  select $Dynamic_select$
  from $Dynamic_from$
  where $Dynamic_where$
 </select>