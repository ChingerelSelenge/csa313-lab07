# BUG-03 — Буруу JSON хүсэлтэд HTTP 200 OK буцаадаг

- Тестер: С. Чингэрэл
- Огноо: 2026-10-24
- Ажигласан цаг: 23:26:29 (+08:00)
- Endpoint: POST http://localhost:3000/api/books
- Severity: Major
- Төлөв: Илэрсэн

## Урьдчилсан нөхцөл

Сервер localhost:3000 дээр ажиллаж байна.

## Давтах алхам

1. Бүрэн бус JSON агуулсан файл үүсгэнэ:

   printf '%s' '{"title":' > evidence/invalid-json-body.txt

2. Хүсэлт илгээнэ:

   curl -sS -i --max-time 10 \
     -X POST http://localhost:3000/api/books \
     -H 'Content-Type: application/json' \
     --data-binary @evidence/invalid-json-body.txt

3. HTTP статус болон response body-г шалгана.

## Хүлээгдэх үр дүн

Сервер JSON синтаксийн алдааг илрүүлж,
HTTP 400 болон алдааны хариу буцаана.

## Бодит үр дүн

HTTP статус:

    HTTP/1.1 200 OK

Response body:

    {"result":"OK"}

## Нөлөө

Хүчингүй хүсэлтийг амжилттай мэт мэдээлдэг.
API хэрэглэгч алдааг зөв таних боломжгүй болно.

## Нотолгоо

- [Curl-ийн бүтэн гаралт](../evidence/api-invalid-json-curl.txt)
- [Илгээсэн body](../evidence/invalid-json-body.txt)
- [Терминалын зураг](../screenshots/30.png)
- [Түүхий тэмдэглэл](../evidence/raw-notes.txt)

## Цагийн тулгалт

Response-ийн Date: Sat, 24 Oct 2026 15:26:29 GMT нь
Asia/Ulaanbaatar бүсийн 23:26:29 цагтай таарна.