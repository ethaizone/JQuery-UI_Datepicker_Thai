#JQuery UI - Datepicker พ.ศ. ไทย
Credit เวอร์ชั่น 1.8.10 โดยคุณ KRISS http://www.anassirk.com/articles/1
สำหรับคนมีปัญหา Datepicker ไม่รองรับ พ.ศ.ไทย 
วิธีใช้อ่านที่เว็บที่มาได้เลยครับ

## Howto manual Patch แบบกากๆ
1. เพิ่ม  isBuddhist: false ต่อท้าย yearSuffix: "" (ค่า Default)
2. สร้างfuncใหม่ _yearOffset: function (a) { if (this._get(a, "isBuddhist")) return 543; return 0 } ต่อท้ายfunc  _daylightSavingAdjust
3. เรียก _yearOffset ต่อหลังจากการเรียกใช้ _getFormatConfig ทุกอัน (เพิ่มเป็น parameter ที่ 4) หรือจำว่าเพิ่มใน func formatDate กับ parseDate ทุกตัวก็ได้
4. เรียก _yearOffset ไปบวกค่า c กับ b ที่อยู่ภายใน <span class="ui-datepicker-year">
5. ใน func formatDate เพิ่ม parameter ตัวที่ 4 แล้ว เป็น z แล้วหาช่วงที่จัดการ ปี เพิ่ม c -= z;
6. ใน func parseDate เพิ่ม parameter ตัวที่ 4 แล้ว เป็น z แล้วหา case "y" ดูเช็คเงื่อนไขตัวแรกเลย ในช่วง True ให้เพิ่ม  +z ต่อท้าย b.getFullYear() 