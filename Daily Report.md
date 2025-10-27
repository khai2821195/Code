# 프로젝트: 카카오 알림톡을 이용한 일일 보고서 자동 발송
## 🎯 목표
n8n과 Notion을 연동하여, 매일 생성되는 Daily Report를 지정된 수신자에게 카카오 알림톡으로 자동 발송하는 워크플로우를 제작합니다.

## 🔗 주요 리소스
*   **n8n 서버:** n8n.math4u.co.kr
*   **Notion 페이지:** https://math4u-edu.notion.site/?pvs=74
*   **카카오 알림톡 API:** (여기에 관련 API 문서나 엔드포인트 추가)

## ✅ 작업 목록 (To-Do List)
-   [ ] Notion에서 Daily Report 데이터 추출하는 방법 정의
-   [ ] n8n 워크플로우 초기 설정
-   [ ] n8n에서 Notion 데이터베이스 읽기 노드 설정
-   [ ] 알림톡으로 보낼 메시지 형식 가공
-   [ ] n8n에서 카카오 알림톡 발송 노드 설정 (API 연동)
-   [ ] 워크플로우 테스트 (테스트 데이터 사용)
-   [ ] 정기 실행을 위한 스케줄 설정 (Cron job)
-   [ ] 오류 처리 및 로깅 설정

## 📝 진행 상황 및 메모
*   (날짜): (여기에 진행 상황, 문제점, 해결 방법 등을 기록)

## ⚙️ 코드 스니펫 / 설정
```json
{
  "nodes": [
    {
      "parameters": {
        "httpMethod": "POST",
        "path": "037ed99a-21d5-40c0-b285-a8362db2ffcc",
        "options": {}
      },
      "type": "n8n-nodes-base.webhook",
      "typeVersion": 2,
      "position": [
        -1616,
        288
      ],
      "id": "8e59d208-55c1-4a06-a120-9da72958ac10",
      "name": "Webhook",
      "webhookId": "037ed99a-21d5-40c0-b285-a8362db2ffcc"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://api-alimtalk.cloud.toast.com/alimtalk/v2.0/appkeys/YuIB1lEDj8vuqME2/messages",
        "authentication": "genericCredentialType",
        "genericAuthType": "httpCustomAuth",
        "sendHeaders": true,
        "headerParameters": {
          "parameters": [
            {
              "name": "Content-Type",
              "value": "application/json;charset=UTF-8"
            },
            {
              "name": "X-Secret-Key",
              "value": "BqbvtCkwuidB28H7cuoYGheQ8SyEWOWw"
            },
            {
              "name": "X-App-Key",
              "value": "YuIB1lEDj8vuqME2"
            }
          ]
        },
        "sendBody": true,
        "bodyParameters": {
          "parameters": [
            {
              "name": "senderKey",
              "value": "7780249050d98477b04a1b589e4794c8ac1f7a78"
            },
            {
              "name": "templateCode",
              "value": "Math4U-edu-2025002"
            },
            {
              "name": "recipientList",
              "value": "={{ $json.finalJson }}"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        -352,
        272
      ],
      "id": "492305e6-6ca1-4a90-800e-7131f3643c4f",
      "name": "HTTP Request",
      "credentials": {
        "httpCustomAuth": {
          "id": "kCMpN7b6ZtLpHBDn",
          "name": "Custom Auth account"
        }
      }
    },
    {
      "parameters": {
        "resource": "databasePage",
        "operation": "update",
        "pageId": {
          "__rl": true,
          "value": "={{ $('Webhook').item.json.body.data.id }}",
          "mode": "id"
        },
        "propertiesUi": {
          "propertyValues": [
            {
              "key": "보고서 발송|checkbox",
              "checkboxValue": true
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.notion",
      "typeVersion": 2.2,
      "position": [
        -112,
        272
      ],
      "id": "ae4482f6-05e6-4b40-aabc-d1fdd52974de",
      "name": "Notion",
      "credentials": {
        "notionApi": {
          "id": "q0CR8H0tBmQfGXS5",
          "name": "Notion account"
        }
      }
    },
    {
      "parameters": {
        "content": "## NHN Cloud API 연결\r\n**Double click** to edit me.\r\n카카오 알림톡 템플릿\r\nmath4u-edu-2025002\r\n\r\n\r\n\r\n\r\n\r\n\r\n\r\n[Guide](https://docs.n8n.io/workflows/sticky-notes/)",
        "height": 420,
        "width": 220
      },
      "type": "n8n-nodes-base.stickyNote",
      "position": [
        -416,
        16
      ],
      "typeVersion": 1,
      "id": "cea0af6a-7ddf-4579-a89e-134fd229e8b3",
      "name": "Sticky Note"
    },
    {
      "parameters": {
        "content": "## Webhook\r\nNotion에서 학습리포트 발송버튼을 클릭하면 Webhook 발동. [Guide](https://docs.n8n.io/workflows/sticky-notes/)",
        "height": 420,
        "width": 204
      },
      "type": "n8n-nodes-base.stickyNote",
      "position": [
        -1664,
        32
      ],
      "typeVersion": 1,
      "id": "c65a3e61-f752-4f0a-b2d8-95b0834e12d8",
      "name": "Sticky Note1"
    },
    {
      "parameters": {
        "height": 420,
        "width": 220
      },
      "type": "n8n-nodes-base.stickyNote",
      "position": [
        -176,
        16
      ],
      "typeVersion": 1,
      "id": "aeec7243-2d64-46dd-8856-47db406cbe78",
      "name": "Sticky Note2"
    },
    {
      "parameters": {
        "jsCode": "const data = $json.body.data.properties;\n\n// 📌 공통 파라미터\nconst commonParams = {\n  url_day: $json.body.data.id,\n  학생이름: data[\"학생이름(S)\"].formula.string,\n  수업날짜: data[\"수업일시(S)\"].formula.string,\n  요일: data[\"요일\"].formula.string,\n  수업회차: data[\"수업회차(S)\"].formula.string,\n  과정: data[\"과정(S)\"].formula.string,\n  수업내용: data[\"진도/수업 내용\"].rollup.array[0]?.rich_text[0]?.plain_text || \"\"\n};\n\n// 📌 수신자 배열\nconst list = [];\n\n// 연락처1 (필수)\nif (data[\"학부모연락처 1(S)\"].formula.string) {\n  list.push({\n    recipientNo: data[\"학부모연락처 1(S)\"].formula.string,\n    templateParameter: commonParams\n  });\n}\n\n// 연락처2 (있으면 추가)\nif (data[\"학부모연락처 2(S)\"].formula.string) {\n  list.push({\n    recipientNo: data[\"학부모연락처 2(S)\"].formula.string,\n    templateParameter: commonParams\n  });\n}\n\n// 📌 숫자만 추출하는 함수\nfunction extractNumber(str) {\n  const match = (str || \"\").match(/\\d+/);\n  return match ? parseInt(match[0], 10) : null;\n}\n\n// 수업회차(S) 숫자\nconst numFromSession = extractNumber(data[\"수업회차(S)\"].formula.string);\n// 과정(S) 숫자\nconst numFromCourse = extractNumber(data[\"과정(S)\"].formula.string);\n\n// 비교 결과\nlet sendPaymentNotice = false;\nif (numFromSession !== null && numFromCourse !== null && numFromSession === numFromCourse) {\n  sendPaymentNotice = true;\n}\n\n// 최종 결과 리턴\nreturn [{\n  json: {\n    recipientList: list,\n    sendPaymentNotice\n  }\n}];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1376,
        288
      ],
      "id": "c0b16b68-a758-4e4b-9ec5-c5e6973f7226",
      "name": "Code"
    },
    {
      "parameters": {
        "height": 420,
        "width": 220
      },
      "type": "n8n-nodes-base.stickyNote",
      "position": [
        -1440,
        32
      ],
      "typeVersion": 1,
      "id": "33d80911-d389-4d80-8e6d-1aa687cefb41",
      "name": "Sticky Note3"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "fde62cf0-98f4-4e3c-9b37-a458a8fc29fc",
              "name": "finalJson",
              "value": "={{ $json.recipientList }}",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        -1152,
        288
      ],
      "id": "d9b13a80-6776-4c17-b5f2-6411cccf1f55",
      "name": "Edit Fields"
    },
    {
      "parameters": {
        "resume": "specificTime",
        "dateTime": "={{ luxon.DateTime.fromISO($workflow.startedAt).setZone('Asia/Seoul').plus({ days: 1 }).set({ hour: 15, minute: 0, second: 0, millisecond: 0 }).toUTC().toFormat('yyyy-MM-dd\'T\'HH:mm:ss') }}"
      },
      "type": "n8n-nodes-base.wait",
      "typeVersion": 1.1,
      "position": [
        -640,
        368
      ],
      "id": "c4f1172e-eb63-4367-9810-4bc5a56eb211",
      "name": "다음날 3시까지 대기1",
      "webhookId": "a8731fd5-336a-4876-80e5-494a92dfe3e1"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 2
          },
          "conditions": [
            {
              "id": "4d236c18-3c4d-4037-ba27-d4b21d4dfa4f",
              "leftValue": "={{ $now.hour }}",
              "rightValue": 22,
              "operator": {
                "type": "number",
                "operation": "smaller",
                "name": "filter.operator.smaller"
              }
            },
            {
              "id": "1861802f-f498-47dd-b191-9d0b2d782390",
              "leftValue": "={{ new Date().getHours() }}",
              "rightValue": 15,
              "operator": {
                "type": "number",
                "operation": "gte"
              }
            },
            {
              "id": "872e221e-9ab7-4b84-8a9d-020c4f287f55",
              "leftValue": "={{ new Date().getHours() }}",
              "rightValue": 22,
              "operator": {
                "type": "number",
                "operation": "lt"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.2,
      "position": [
        -928,
        288
      ],
      "id": "6138e639-95cf-4b52-8378-8f18fd1d0386",
      "name": "3시에서 10시사이 인가?"
    }
  ],
  "connections": {
    "Webhook": {
      "main": [
        [
          {
            "node": "Code",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "HTTP Request": {
      "main": [
        [
          {
            "node": "Notion",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Code": {
      "main": [
        [
          {
            "node": "Edit Fields",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Edit Fields": {
      "main": [
        [
          {
            "node": "3시에서 10시사이 인가?",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "다음날 3시까지 대기1": {
      "main": [
        [
          {
            "node": "HTTP Request",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "3시에서 10시사이 인가?": {
      "main": [
        [
          {
            "node": "HTTP Request",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "다음날 3시까지 대기1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "pinData": {},
  "meta": {
    "instanceId": "42c40a3b0e702882e5efb1e0bfa1f181054ea2f804caec8b597ee1dd0e202dec"
  }
}
```