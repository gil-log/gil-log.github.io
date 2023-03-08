---
title: "[DDaJa] REST API 작성하기"
last_modified_at: 2021-06-22T23:41
categories: 
  - sideproject
tags: 
  - 'API' 
  - 'DDaJa' 
  - 'REST API'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
> 진짜 개발자는 엑셀부터 키는 개발자야. <br>
by. TigerDragon

이번에는 DDaJa에서 **Spring Boot로 구성된 BackEnd Server**에서 제공할 **REST API를 작성**해보려 한다.

**Node Front End로 구성된 Vue 서버에서 Data 송,수신**을 위한 **API가 필요**하고,

**필요한 기능들에 맞게 먼저 기능들을 `B`와 함께 작성**해놓았다.

**아래와 같은 순서**로 **최종적으로 API를 개발**해보려한다.

1. **필요한 기능들에서 자원과 관련된 기능들을 정리, HTTP Method 정의**
2. **DB Schema와 함께 URL 예시 생성**
3. **Spring Boot Controller Swagger Annotation으로 Code 작성**
4. **DB Schema 최종 완성 시 Service, Repository 등 Business Logic과 함께 개발**

---

# Functions Needed - User

# Main

- 자격증 목록 / 정보 (GET)
- DDaJa 사용자 사용 현황 (GET)
- 자격증들 시험 D-Day 목록 정보 (GET)
- 공지사항 목록 (GET)

## 공지사항
- 공지사항 상세 정보 (GET)

# 한 문제씩 풀기

- 응시 회차수 정보 (필기, 실기 유무) (GET)
- 응시 회차별 사용자 진행율 (GET)
- 해당 응시 회차 문제 목록(정답 포함) (GET)
- 시험 응시 완료 시 사용자의 제출 답안 저장 (POST)

# 토론

- 사용자 토론 의견 저장 (POST)
- 해당 문제의 사용자들이 작성한 토론 정보 댓글 (GET)
- 사용자 본인의 댓글 삭제 (DELETE)
- 좋아요, 싫어요, 댓글 수정 및 생성 (PATCH, PUT)

# 단어 암기

- 단어 암기 목록 (GET)
- 해당 암기의 문제 목록 (GET)

# 모의고사

- 응시 회차수 정보 (필기, 실기 유무) (GET)
- 응시 회차별 사용자 진행율 (GET)
- 해당 응시 회차 문제 목록(정답 포함) (GET)
- 시험 응시 완료 시 사용자의 제출 답안 저장 (POST)

# 합격후기

- 합격 후기 목록(상세 정보 포함) (GET)
- 합격 후기 저장 (POST)
- 합격 후기 수정 (PUT)

# 자격증 정보

- 자격증 일정 (GET)
- 자격증 과목 목록 (GET)
- 자격증 합격 현황 (GET)
- 자격증 응시 자격 (GET)
- 합격 기준 정보 (GET)


---

# REST API URL 설계

먼저 사용자에게 필요한 기능들은 위와 같이 간략하게 정리하였다.

이제 실제 Schema와 함께 예시 URL을 작성하려 한다.

먼저 아직 수정중이긴 하지만 DB Schema는 아래와 같다.

![](https://images.velog.io/images/gillog/post/b93d0a1d-7b5b-4f9a-9bea-be206b1a95d1/image.png)
_ 아 너무 작은데 ㅋㅋ _

## Main

### 자격증 목록 / 정보 (GET)

```python
GET api.ddaja.com/licenses 

HTTP/1.1 200 OK
Link: 
 <https://api.ddaja.com/licenses?offset=10&limit=10>; rel="next"
, <https://api.ddaja.com/licenses?offset=50&limit=10>; rel="last"
, <https://api.ddaja.com/licenses?offset=0&limit=10>; rel="first"
, <https://api.ddaja.com/licenses?offset=0&limit=0>; rel="prev"
{
    "total_cnt": 120
    , "items": [
    	{
            "info": {
                "seq": 1
                , "name": "정보처리기사"
                , "ins_date": "2021-05-30"
                , "agency": "Q-Net"
                , "pass_score": 70
            }
            , "links": [
                {
                    "rel": "self",
                    "href": "http://api.ddaja.com/licenses/1",
                    "method": "GET"
                },
                {
                    "rel": "delete",
                    "href": "http://api.ddaja.com/licenses/1",
                    "method": "DELETE"
                }
            ]
        }
        , {
            "info": {
                "seq": 2
                , "name": "빅데이터기사"
                , "ins_date": "2021-04-29"
                , "agency": "Q-Net"
                , "pass_score": 70
            }
            , "links": [
                {
                    "rel": "self",
                    "href": "http://api.ddaja.com/licenses/2",
                    "method": "GET"
                },
                {
                    "rel": "delete",
                    "href": "http://api.ddaja.com/licenses/2",
                    "method": "DELETE"
                }
            ]
        }
        , {
            "info": {
                "seq": 3
                , "name": "컴퓨터활용능력1급"
                , "ins_date": "2021-05-10"
                , "agency": "Q-Net"
                , "pass_score": 70
            }
            , "links": [
                {
                    "rel": "self",
                    "href": "http://api.ddaja.com/licenses/3",
                    "method": "GET"
                },
                {
                    "rel": "delete",
                    "href": "http://api.ddaja.com/licenses/3",
                    "method": "DELETE"
                }
            ]
        }
        , ...
    ]
}

GET api.ddaja.com/licenses/1

HTTP/1.1 200 OK
{
    "info": {
        "seq": 1
        , "name": "정보처리기사"
        , "ins_date": "2021-04-29"
        , "agency": "Q-Net"
        , "pass_score": 70
    }
    , "links": [
        {
            "rel": "self",
            "href": "http://api.ddaja.com/licenses/3",
            "method": "GET"
        },
        {
            "rel": "delete",
            "href": "http://api.ddaja.com/licenses/3",
            "method": "DELETE"
        }
    ]
}

GET api.ddaja.com/licenses/subjects

HTTP/1.1 200 OK
Link: 
 <https://api.ddaja.com/licenses/subjects?offset=10&limit=10>; rel="next"
, <https://api.ddaja.com/licenses/subjects?offset=50&limit=10>; rel="last"
, <https://api.ddaja.com/licenses/subjects?offset=0&limit=10>; rel="first"
, <https://api.ddaja.com/licenses/subjects?offset=0&limit=0>; rel="prev"
{
    "licenses": [
    	{
            "info": {
                "seq": 1
                , "name": "정보처리기사"
                , "subjects": [
                    {
                        "info": {
                            "seq": 1
                            , "name": "전자계산기구조"
                            , "ins_date": "2021-06-01"
                            , "pass_score": 60
                        }
                    }
                    ,{
                        "info": {
                            "seq": 2
                            , "name": "컴퓨터공학기초"
                            , "ins_date": "2021-06-03"
                            , "pass_score": 60
                        }
                    }
                    ,{
                        "info": {
                            "seq": 3
                            , "name": "프로그램이론"
                            , "ins_date": "2021-06-02"
                            , "pass_score": 60
                        }
                    }
                    ,{
                        "info": {
                            "seq": 4
                            , "name": "정보보안"
                            , "ins_date": "2021-06-04"
                            , "pass_score": 60
                        }
                    }
                ]
            }
        }
        , {
            "info": {
                "seq": 2
                , "name": "빅데이터기사"
                , "subjects": [
                    {
                        "info": {
                            "seq": 5
                            , "name": "빅데이터이론"
                            , "ins_date": "2021-06-01"
                            , "pass_score": 60
                        }
                    }
                    ,{
                        "info": {
                            "seq": 6
                            , "name": "알고리즘"
                            , "ins_date": "2021-06-03"
                            , "pass_score": 60
                        }
                    }
                    ,{
                        "info": {
                            "seq": 7
                            , "name": "데이터"
                            , "ins_date": "2021-06-02"
                            , "pass_score": 60
                        }
                    }
                    ,{
                        "info": {
                            "seq": 8
                            , "name": "빅데이터개론"
                            , "ins_date": "2021-06-04"
                            , "pass_score": 60
                        }
                    }
                ]
            }
        }
}
```



### DDaJa 사용자 사용 현황 (GET)

```python
GET /users/status

HTTP/1.1 200 OK
Link: 
 <https://api.ddaja.com/users/status?offset=10&limit=10>; rel="next"
, <https://api.ddaja.com/users/status?offset=50&limit=10>; rel="last"
, <https://api.ddaja.com/users/status?offset=0&limit=10>; rel="first"
, <https://api.ddaja.com/users/status?offset=0&limit=0>; rel="prev"
{
    "users": [
        {
            "user": {
                "seq": 1
                , "name": "gil"
                , "id": "gillog"
            }
            , "state": {
                "type": "one_question"
                , "progress": "ing"
                , "detail": ""
            }
        }
        , {
            "user": {
                "seq": 2
                , "name": "bin"
                , "id": "binsoo"
            }
            , "state": {
                "type": "round_question"
                , "progress": "done"
                , "detail": "perfect"
            }
        }
        , ...
    ]
}
```

### 자격증들 시험 D-Day 목록 정보 (GET)

```python
GET /licenses/exams/
HTTP/1.1 200 OK
Link: 
 <https://api.ddaja.com/licenses/dday?offset=10&limit=10>; rel="next"
, <https://api.ddaja.com/licenses/dday?offset=50&limit=10>; rel="last"
, <https://api.ddaja.com/licenses/dday?offset=0&limit=10>; rel="first"
, <https://api.ddaja.com/licenses/dday?offset=0&limit=0>; rel="prev"

{
    "licenses": [
        {
            "license": {
                "seq": 1
                , "name": "정보처리기사"
                , "exams": [
                    {
                        "exam": {
                            "seq": 1
                            , "name": "정보처리기사 실기 2회"
                            , "date": "2021-06-01"
                            , "info": "펜 볼펜 등 필수 지참"
                        }
                    }
                    , {
                        "exam": {
                            "seq": 2
                            , "name": "정보처리기사 필기 3회"
                            , "date": "2021-09-01"
                            , "info": "펜 볼펜 등 필수 지참"
                        }
                    }
                    , {
                        "exam": {
                            "seq": 3
                            , "name": "정보처리기사 실기 3회"
                            , "date": "2021-12-01"
                            , "info": "펜 볼펜 등 필수 지참"
                        }
                    }
                ]
            }
        , ...
    ]
}
```
- 공지사항 목록 (GET)

```python
GET /notice
HTTP/1.1 200 OK
Link: 
 <https://api.ddaja.com/notice?offset=10&limit=10>; rel="next"
, <https://api.ddaja.com/notice?offset=50&limit=10>; rel="last"
, <https://api.ddaja.com/notice?offset=0&limit=10>; rel="first"
, <https://api.ddaja.com/notice?offset=0&limit=0>; rel="prev"

{
    "notice": [
        {
            "seq": 1
            , "title": "공지사항 1번글"
            , "content": "공지사항 본문 ~~~"
            , "ins_date": "2021-06-25"
            , "in_use": 0
        }
        , {
            "seq": 2
            , "title": "공지사항 2번글"
            , "content": "공지사항 본문 !!~~~"
            , "ins_date": "2021-06-26"
            , "in_use": 1
        }
        , ...
    ]
}

GET /notice
```