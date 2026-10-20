# BUG-03: Бүрэн бус JSON хүсэлтэд API HTTP 200 болон OK буцаадаг

## Орчин

- Апп: Уншлагын жагсаалт v2.0
- Node.js: v20.19.2
- Git: 2.47.3
- curl: 8.14.1
- browser: Mozilla Firefox 153.4.0esr
- Үйлдлийн систем: Linux
- Огноо: 2026-10-20

## Урьдчилсан нөхцөл

Дахин шалгахдаа `node app.js`-ийг шинээр асааж, http://localhost:3000 хуудсыг шинэчилнэ. Анхны 3 номтой төлөвөөс эхэлнэ.

## Дахин гаргах алхам

1. http://localhost:3000 хаягийг нээнэ.
2. `evidence/invalid-json-body.txt` файлд `{"title":` гэсэн бүрэн бус JSON хадгална.
3. Доорх curl командыг ажиллуулна.
4. HTTP статус болон response body-г ажиглана.

```bash
printf '%s' '{"title":' > evidence/invalid-json-body.txt
curl -i --max-time 10 \
  -X POST http://localhost:3000/api/books \
  -H 'Content-Type: application/json' \
  --data-binary @evidence/invalid-json-body.txt \
  2>&1 | tee evidence/bug-03-recheck-curl.txt
```

Дахин шалгалтыг тусдаа файлд хадгалж, session-ийн эх нотолгоог дарахгүй.

## Хүлээгдэж байсан үр дүн

Спецификацийн дагуу буруу JSON биетэй POST хүсэлтэд HTTP 400 буцаана.

## Бодит үр дүн

HTTP/1.1 200 OK болон `{"result":"OK"}` буцсан.

## Ноцтой байдал

Major — Формат буруу хүсэлтийг амжилттай гэж мэдээлж байна.

## Нотолгоо

[Session-ийн curl гаралт](../evidence/bug-03-curl.txt)

[Хүсэлтийн body](../evidence/invalid-json-body.txt)

screenshots/bug-03-185344-01.png

## Олдсон аялал

Saboteur — санал болгосон ангилал; өөрийн session-д бодитоор ашигласан аялалтай тулгаж баталгаажуулна.

## Нэмэлт тэмдэглэл

Ном үнэхээр нэмэгдсэн эсэхийг энэ HTTP гаралтаас дүгнээгүй. Session evidence-ийн Date нь 2026-10-20 10:52:04 GMT буюу +08:00 цагаар 18:52:04.