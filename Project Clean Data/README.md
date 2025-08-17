# **Messy IMDB Data Cleaning**

## Introduction
โปรเจกต์นี้เป็นโปรเจกต์ที่นำ Dataset จาก Kaggle ที่มีความยุ่งเหยิงของข้อมูลมาทำความสะอาดเมื่อตรวจสอบแล้วว่าข้อมูลมีความถูกต้องและสะอาดมากพอ ขั้นต่อคือจะนำมาทำการวิเคราะห์เพื่อหาข้อมูลเชิงลึกต่อไป

### การทำความสะอาดข้อมูลของ Dataset

 1. ย้ายชื่อ Columns ให้อยู่ในตำแหน่งที่ถูกต้อง

 2. ลบ Row ที่เป็น Blank Row Value

 3. ลบ Column ที่เป็น Blank Column Value

 4. แก้ไขชื่อ Columns ให้ถูกต้อง

 5. แก้ไขข้อมูลของชื่อหนังที่พิมพ์ตัวอักษรผิด หรือชื่อผิด

 6. แก้ไขข้อมูลวันที่ ที่ผิดรูปแบบ Format

 7. แก้ไขข้อมูลความยาวเวลาของหนังโดยเพิ่มข้อมูลที่ขาดหายให้ไปถูกต้อง

 8. แก้ไขชื่อประเทศให้ถูกต้อง

 9. แก้ไขประเภท Content Rating ให้ถูกต้อง

 10. แก้ไขข้อมูลของผู้กำกับ

 11. แก้ไขข้อมูลรายได้ของหนังที่มีตัวอักษรผสมนอกจากตัวเลข

 12. แก้ไขรูปแบบการแสดงตัวเลขข้อมูลของผลโหวตให้ถูกต้อง

13. แก้ไขข้อมูลคะแนน Rating ให้ถูกต้องเนื่องจากมีตัวอักษรอื่นผสม

### สกิลของ Excel ที่ใช้
 * 🔍 Power Query

### Data IMDB Dataset
Dataset ที่ใช้ในโปรเจกต์นี้ประกอบไปด้วยข้อมูลรายชื่อของหนังจำนวน 100 รายการซึ่งประกอบไปด้วยข้อมูลที่ไม่สะอาดเหมาะแก่การนำมาฝึกฝนและทำโปรเจกต์เกี่ยวกับการทำความสะอาดข้อมูล โดยสามารถเข้าถึงข้อมูลนี้ได้ผ่านเว็บไซต์ของ Kaggle [messy-imdb-dataset](https://www.kaggle.com/datasets/davidfuenteherraiz/messy-imdb-dataset)

# ✏️ เริ่มต้นการทำความสะอาดข้อมูล

จากรูปด้านล่างเป็นรูปของข้อมูลที่ยังไม่ได้ทำความสะอาดซึ่งมีความยุ่งเหยิงของข้อมูลหลากหลายรูปแบบ

![Messy Data](./assets/Messy%20Data.png)

### 📄 **Promote First Row as Header**

![Before Promote Headers](./assets/Before%20Make%20First%20Rows%20as%20Header.png)

![Promote Headers](./assets/After%20First%20Rows%20as%20Header.png)

จะสังเกตได้ว่าชื่อของ `Column` ณ ตอนนี้จะเป็นชื่อ `Column1, Column2, Column3` เรียงต่อไปจนไปถึง `Column` สุดท้าย ถ้าดูให้ดีๆแล้วชื่อของ `Column` จริงๆนั้นจะอยู่ใน `Rows` แรกสุด ฉะนั้นแล้วเราต้องทำการ Promote Header เพื่อให้มีชื่อ `Column` ที่ถูกต้องโดยใช้คำสั่ง `Use First Row as Headers` ในแถบ `Home` บริเวณ menu `Transform`

### 📄 **Delete Blank Rows**

![Row Blank Value](./assets/Row%20Blank.png)

หากสังเกตจากรูปด้านบนแล้วจะพบว่าแถวที่ 14 ของข้อมูลนั้นเป็นค่าว่างทั้งหมดจึงต้องทำการลบแถวนี้ออกเพราะ `Blank Row` (แถวว่าง) มักจะถือว่าเป็น ข้อมูลที่ไม่มีค่า หรือ ไม่สมบูรณ์ ไม่ก่อให้เกิดประโยชน์ถ้าเกิดนำข้อมูลไปวิเคราะห์ต่อ


### 📄 **Delete Blank Columns**

![Column Blank Value](./assets/Column%20Blank.png)

เนื่องจาก `Column` เป็น `Blank Column` จึงไม่มีประโยชน์ที่จะต้องเก็บไว้เช่นเดียวกับ `Blank Row` เพราะถือว่าเป็น ข้อมูลที่ไม่มีค่า หรือ ไม่สมบูรณ์

### 📄 **Rename Columns**

![Rename Typo Columns Name](./assets/Before%20Change%20Columns%20name.png)

เนื่องจากชื่อ Columns บางอันมีการพิมพ์ชื่อผิดหรือมีตัวอักษรที่ไม่ควรจะมีอยู่ซึ่งทำให้ความหมายผิดเพี้ยนไป จึงต้องมีการเปลี่ยนชื่อ Columns ให้ถููกต้อง เพื่อที่จะได้สื่อความหมายของข้อมูลให้ถูกต้องด้วยเช่นกัน

### 📄 **Correct Movie Title Typos**

จากการตรวจสอบข้อมูลเบื้องต้นพบว่ามีรายชื่อหนังบางอันที่มีข้อมูลที่ผิดจากความเป็นจริงจึงต้องทำการแก้ไขชื่อหนังให้ถูกต้อง

#### รายชื่อหนังที่ผิด

1. **LÃ©on**
2. **WALLÂ·E**
3. **Le fabuleux destin d'AmÃ©lie Poulain**
4. **Per qualche dollaro in piÃ¹**
5. **La vita B9 bella**

````
= Table.ReplaceValue(
    #"Replaced Errors",
    each [Original title],
    each if [Original title] = "Le fabuleux destin d'AmÃ©lie Poulain" then "Le Fabuleux Destin d'Amélie Poulain"
    else if [Original title] = "Per qualche dollaro in piÃ¹" then "Per qualche dollaro in più"
    else if [Original title] = "WALLÂ·E" then "WALL·E"
    else if [Original title] = "La vita B9 bella" then "La vita è bella"
    else if [Original title] = "LÃ©on" then "Léon: The Professional"
    else [Original title], Replacer.ReplaceText, {"Original title"}
)
````
* จาก Code M Language ด้านบนจะใช้วิธีการ Replace Value ในรูปแบบ Conditional เพื่อแก้ไขข้อมูลตามเงื่อนไขจากรายชื่อหนังที่ผิดที่เราหามาได้ มาแก้ไขให้ถูกต้อง

### 📄 **Standardize Date Format**

รูปแบบวันที่ในข้อมูลนั้นมีข้อมูลบางตัวที่ยังผิดอยู่และไม่ตรงกับรูปแบบที่ควรจะเป็น จึงต้องทำการแก้ไขให้ถูกต้อง เพื่อที่จะสามารถนำข้อมูลไปใช้ประโยชน์ในการวิเคราะห์ได้

#### รายการข้อมูลที่ผิด

1. **09 21 1972**
2. **23 -07-2008**
3. **22 Feb 04**
4. **10-29-99**
5. **23rd December of 1966**
6. **01/16-03**
7. **18/11/1976**
8. **21-11-46**
9. **The 6th of marzo, year 1951**
10. **1984-02-34**
11. **1976-13-24**

จากข้อมูลทางด้านบนมีข้อมูลที่ผิดอยู่หลายรูปแบบ และมีข้อมูลที่ถูกต้องตามรูปแบบแล้วแต่ดันเป็นข้อมูลที่ไม่ถูกต้อง

````
= Table.ReplaceValue(
    #"Null Value Replaced",
    each [Release year],
    each if [Release year] = "09 21 1972" then "1972-09-21"
    else if [Release year] = "23 -07-2008" then "2008-07-23"
    else if [Release year] = "22 Feb 04" then "2004-02-22"
    else if [Release year] = "10-29-99" then "1999-10-29"
    else if [Release year] = "23rd December of 1966" then "1996-12-23"
    else if [Release year] = "01/16-03" then "2003-01-16"
    else if [Release year] = "18/11/1976" then "1976-11-18"
    else if [Release year] = "21-11-46" then "1946-11-21"
    else if [Release year] = "The 6th of marzo, year 1951" then "1951-03-06"
    else if [Release year] = "1984-02-34" then "1983-12-09"
    else if [Release year] = "1976-13-24" then "1976-02-08"
    else [Release year], Replacer.ReplaceText, {"Release year"}
)
````
* จาก Code M Language ทางด้านบน เนื่องจากมีรูปแบบข้อมูลที่ผิดหลายรูปแบบจึงทำการแก้ไขข้อมูลโดยการ Replace Value แบบ Conditional เพื่อแก้ไขข้อมูลให้ถูกต้องในครั้งเดียวโดยไม่ต้อง Replace Value หลายครั้ง

### 📄 **Fill Missing Duration Data**

![Duration Before Fix](./assets/Duration%20Before%20Fix.png)
![Duration Before Fix](./assets/Fix%20Duration.png)

จากรูปทางด้านบนเป็นรูปของข้อมูล Column Duration ซึ่งเป็นความยาวของหนังแต่ละเรื่อง รูปทางด้านซ้ายเป็นรูปที่ยังไม่ได้ทำการแก้ไขสังเกตได้ว่ายังมี Null Value อยู่ จำเป็นต้องเอาข้อมูลที่ถูกต้องมาทำการใส่เข้าไปเพื่อให้ข้อมูลถูกต้องครบถ้วน

````
= let
    Source = #"Replaced Value",
    UpdatedDuration = Table.AddColumn(
        Source,
        "Custom",
        each if [IMDB title ID] = "tt0110912" then 154
        else if [IMDB title ID] = "tt0108052" then 195
        else if [IMDB title ID] = "tt0137523" then 139
        else if [IMDB title ID] = "tt0133093" then 136
        else if [IMDB title ID] = "tt0080684" then 124
        else if [IMDB title ID] = "tt0073486" then 133
        else [Duration],
        Int64.Type
    ),
    RemoveOld = Table.RemoveColumns(UpdatedDuration, {"Duration"}),
    Renamed = Table.RenameColumns(RemoveOld, {{"Custom", "Duration"}})
in
    Renamed
````

* จาก Code M Language ทางด้านบน จะใช้เป็นวิธีการสร้าง Conditional Custom Column ขึ้นมาและทำการแก้ไขข้อมูล โดยใช้ Column `IMDB title ID` เป็นข้อมูลในการหาแถวของ Column `Duration` เพื่อแก้ไขข้อมูลลงไปใน Column `Duration`

### 📄 **Standardize Country Names**

![Before Fix Country Name](./assets/Country%20Name%20Before%20Fix.png)
![Fix Country Name](./assets/Fix%20Country%20Name.png)

จากรูปจะเห็นได้ว่ามีข้อมูลชื่อประเทศที่ไม่ตรงกันเช่น New Zealand มีทั้ง New Zesland, New Zeland และ New Zealand ซึ่งต้องแก้ไขให้เป็นไปในรูปแบบที่ถูกต้องและรูปแบบเดียวกัน

````
= Table.ReplaceValue(
    #"Fix Director Name Typo",
    each [Country],
    each if [Country] = "US" or [Country] = "US." then "USA"
    else if [Country] = "New Zesland" or [Country] = "New Zeland" then "New Zealand"
    else if [Country] = "Italy1" then "Italy"
    else [Country], Replacer.ReplaceText,{"Country"}
)
````

* แก้ไขด้วยการใช้ Conditional Replace Value เพื่อแก้ไขข้อมูลที่ไม่ถูกต้องหลายๆอันได้ภายในครั้งเดียว

### 📄 **Standardize Content Rating Format**

![Before Fix Content Rating](./assets/Content%20Rating%20Before%20Fix.png)
![Fix Content Rating](./assets/Fix%20Content%20Rating.png)

จากรูปจะเห็นได้ว่ามีรูปแบบข้อมูลหลายอย่าง แต่จะมีข้อมูลที่เราต้องแก้ไขคือ `Not Rated, Approved, Unrated, #N/A` ซึ่ง `Not Rated, Unrated, #N/A` จะแก้ไขเป็น `NR (Not Rated)` ส่วน `Approved` จะต้องหาข้อมูล Content Rating ที่ถูกต้องของหนังมาใส่

````
= Table.ReplaceValue(
    #"Income Replaced Value",
    each [Content Rating],
    each if [Content Rating] = "#N/A" then "NR"
    else if [Content Rating] = "Not Rated" then "NR"
    else if [Content Rating] = "Unrated" then "NR"
    else if [Content Rating] = "Approved" then "R"
    else [Content Rating],Replacer.ReplaceText,{"Content Rating"}
)
````
* จะใช้ Conditional Replace Value แบบเดียวกับข้อที่ผ่านๆมา และเนื่องจาก `Approved` มีแค่ค่าเดียวจึงไม่จำเป็นต้องกังวลว่าข้อมูล Content Rating ของหนังอื่นๆที่อาจจะมีค่า `Approved` จะไม่ถูกต้อง

### 📄 **Fix Typo in Director Name**

จากการตรวจสอบเบื้องต้นจะมีรายชื่อผู้กำกับบางคนที่มีข้อมูลที่ผิดพลาดอยู่ และหนังบางเรื่องจะมีผู้กำกับถึง 2 คนด้วยกันจึงต้องทำการแก้ไขข้อมูลให้สอดคล้องถูกต้อง

![Director Before Fix](./assets/Director%20Name%20Before%20fix.png)
![Director Fix](./assets/Fix%20Director%20Name.png)

รายชื่อผู้กำกับที่มีสองคนจะถูกคันด้วย `", "` ดังนั้นจึงเปลี่ยนเป็นสัญลักษณ์ `" & "` เพื่อให้เหมาะสมและถูกต้อง และจะมีรายชื่อผู้กำกับที่ผิดพลาดจึงต้องทำการแก้ไขชื่อให้ถูกต้องคือ `Ã‰ric Toledano และ KÃ¡tia Lund` ชื่อของผู้กำกับทั้งสองคนนี้ที่ถูกต้องคือ `Éric Toledano และ Kátia Lund`

* การเปลี่ยน `", "` ไปเป็น `" & "` ใน Column `Director` จะใช้การ Replace Value ปกติ
````
 = Table.ReplaceValue(#"Fix Country Typo",", "," & ",Replacer.ReplaceText,{"Director"})
 ````

 * การแก้ไขชื่อผู้กำกับใน Column `Director` จะใช้ Conditional Replace Value
 ````
 = Table.ReplaceValue(
    #"Fix Movie Title Typo",
    each [Director],
    each if [Director] = "Olivier Nakache, Ã‰ric Toledano" then "Olivier Nakache, Éric Toledano"
    else if [Director] = "Fernando Meirelles, KÃ¡tia Lund" then "Fernando Meirelles, Kátia Lund"
    else [Director], Replacer.ReplaceText, {"Director"}
)
````


### 📄 **Standardize Income Format**

![Before Fix Income](./assets/Income%20Before%20Fix.png)

ข้อมูลรายได้ของหนังหรือ Column `Income` จะทำการแก้ไขข้อมูลโดยการเอาสัญลักษณ์ `"$", ",", "o"` ออกเพื่อให้เหลือแต่ตัวเลขเท่านั้น

````
= let
    Source = #"Remove ""."" From Vote Values",
    ToReplace = {"$", "o", ","},
    Cleaned = List.Accumulate(
        ToReplace,
        Source,
        (state, current) => Table.ReplaceValue(state, current, "", Replacer.ReplaceText, {"Income"})
    )
in
    Cleaned
````

### 📄 **Standardize Votes Format**

![Vote Value Before Fix](./assets/Votes%20Before%20Fix.png)
![Vote Value Fix](./assets/Fix%20votes.png)

ข้อมูลใน Column `Votes` เป็นตัวเลขที่คันด้วย `"."` ทุกๆ 3 หลัก ซึ่งเป็นรูปแบบที่ไม่ถูกต้องจึงต้องทำการแก้ไขเอาออกโดยใช้การ Replace Value ปกติ

````
= Table.ReplaceValue(#"Removed Blank Columns",".","",Replacer.ReplaceText,{" Votes "})
````

### 📄 **Standardize Score Format**

![Score Before Fix](./assets/Score%20Before%20Fix.png)
![Score Fix](./assets/Fix%20Score.png)

จากรูปข้อมูลของ Column `Score` มีรูปแบบที่ผิดหลายรูปแบบต้องแก้ไขให้เป็นตัวเลขในรูปแบบทศนิยม 1 ตำแหน่ง จาก Code M Language ด้านล่าง จะเป็นการแก้ไขข้อมูลแบบ Conditional Replace Value ให้กับข้อมูลที่ผิดรูปแบบเพื่อจะได้ไม่ต้องทำการใช้ Replace Value หลายๆครั้ง จะสังเกตได้ว่าจะมีข้อมูล 1 ตัว คือ `08.9` ที่ไม่ได้ทำการแก้ไข ข้อมูลตัวนี้หากทำการเปลี่ยน `Data Type` ของข้อมูลให้เป็นตัวเลข (Number) เลข 0 ก็จะหายไปจึงไม่ได้ทำการแก้ไข

````
= Table.ReplaceValue(
    #"Fix Format Date Value",
    each [Score],
    each if [Score] = "9." then "9.0"
    else if [Score] = "8,9f" then "8.9"
    else if [Score] = "8..8" then "8.8"
    else if [Score] = "8:8" then "8.8"
    else if [Score] = "++8.7" then "8.7"
    else if [Score] = "8.7." then "8.7"
    else if [Score] = "8,7e-0" then "8.7"
    else if [Score] = "9,.0" then "9.0"
    else if [Score] = "8,6" then "8.6"
    else [Score], Replacer.ReplaceText, {"Score"}
)
````
## Conclusion

การทำความสะอาดข้อมูลเป็นจุดเริ่มต้นที่สำคัญก่อนที่จะนำข้อมูลไปใช้ในการวิเคราะห์หาข้อมูลเชิงลึก ถ้าหากเราทำความสะอาดข้อมูลได้เป็นอย่างดีข้อมูลที่เราได้ทำการแก้ไขนี้ก็จะสามารถนำไปใช้ให้เกิดประโยชน์ได้อย่างมหาศาล เพราะความถูกต้องของข้อมูลนั้นเป็นสิ่งที่สำคัญหากได้ข้อมูลที่ผิดพลาดการนำไปวิเคราะห์ต่อนั้นก็จะไม่เกิดประโยชน์





