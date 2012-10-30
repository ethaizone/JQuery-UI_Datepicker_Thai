#JQuery UI - Datepicker พ.ศ. ไทย
Credit เวอร์ชั่น 1.8.10 โดยคุณ KRISS http://www.anassirk.com/articles/1
สำหรับคนมีปัญหา Datepicker ไม่รองรับ พ.ศ.ไทย 
วิธีใช้อ่านที่เว็บที่มาได้เลยครับ

เฉพาะรุ่น 1.8.16 เท่านั้นที่ผมทดสอบแล้ว นอกนั้นเป็นการแก้ไขโดยไม่ได้ทดสอบแบบละเอียด ถ้าพบปัญหาให้ทำตามข้างล่าง

## เรามีอะไรในตอนนี้?
- JQuery UI v1.8.16
- JQuery UI v1.8.24
- JQuery UI v1.9.1

## Howto manual Patch แบบกากๆ
1. เพิ่ม  isBuddhist: false ต่อท้าย yearSuffix: "" (ค่า Default)
2. สร้างfuncใหม่ _yearOffset: function (a) { if (this._get(a, "isBuddhist")) return 543; return 0 } ต่อท้ายfunc  _daylightSavingAdjust
3. เรียก _yearOffset ต่อหลังจากการเรียกใช้ _getFormatConfig ทุกอัน (เพิ่มเป็น parameter ที่ 4) หรือจำว่าเพิ่มใน func formatDate กับ parseDate ทุกตัวก็ได้
4. เรียก _yearOffset ไปบวกค่า c กับ b ที่อยู่ภายใน <span class="ui-datepicker-year">
5. ใน func parseDate เพิ่ม parameter ตัวที่ 4 แล้ว เป็น z และเพิ่ม c -= z; ต่อท้าย ?0:-100)); 
*ในกรณีตัวแปรปีเป็น c (ค่า parameterที่ 3 ของ parseDate)*
6. ใน func formatDate เพิ่ม parameter ตัวที่ 4 แล้ว เป็น z และหา case "y" ดูเช็คเงื่อนไขตัวแรกเลย ในช่วง True ให้เพิ่ม  +z ต่อท้าย b.getFullYear()  ที่อยู่หลัง if ก่อน else (:)
*ในกรณีตัวแปรเดือน เป็น b (ค่า parameterที่ 2 ของ formatDate)*

## พบปัญหา?
สร้าง Issue ใหม่ในระบบ Github แล้วแจ้งพร้อมคัดลอก error ที่ขึ้นจาก console หรือ error log ของ web browser พร้อมบอกรุ่นที่ใช้งานในขณะนั้นด้วย (รุ่น Browser/JQuery และ JQuery UI)

## Memo (for Editplus - Find with Regex)
**Step: 3**
Find:
	,((\$|this)(\.[a-z]+)?)\._getFormatConfig\(([a-z])\)
Replace:
	,\1._getFormatConfig(\4),\1._yearOffset(\4)

**Step: 4**
Find:
	('|")\+([a-z])\+"</(span|option)>";
	([a-z]) as $1

and Find: 
	[a-z].yearshtml
	[a-z] as $2

and make
	+($2+this._yearOffset($2))+"</