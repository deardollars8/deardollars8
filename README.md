
var line_api_token = "โทเค็นนะจ๊ะ";

function doGet(e) {
  
  var deadline = e.parameter.deadline;
  var contents = e.parameter.contents;

  var sheet = SpreadsheetApp.getActiveSheet();
  sheet.insertRowBefore(1);
  sheet.getRange('A1').setValue(deadline);
  sheet.getRange('B1').setValue(contents);
}

function myFunction() {
  var sheet = SpreadsheetApp.getActiveSheet();
  var last_row = sheet.getLastRow();
   for(var i = 1; i <= last_row; i++) {
    var target_deadline = new Date(sheet.getRange('A' + i).getValue());
    var tommorow = new Date();
    tommorow.setDate(tommorow.getDate()+1);//สร้างวันที่ของพรุ่งนี้

    //ถ้าถึงกำหนด DeadLine จะทำงาน
    if(tommorow.getYear() == target_deadline.getYear() && tommorow.getMonth() == target_deadline.getMonth() && tommorow.getDate() == target_deadline.getDate()) {
     //ใส่เนื้อหาเมื่อถึงกำหนด
      var target_contens = sheet.getRange('B' + i).getValue();


     
      var options = 
          {
            "method" : "post",
            "headers" : {'Authorization': "Bearer " + line_api_token},
            "payload" : {"message" : target_contens}
          };
      UrlFetchApp.fetch("https://notify-api.line.me/api/notify", options);


      //จะทำการลบทีละแถว
      sheet.deleteRow(i);
      i--;//แถวจะลดลง
      last_row--;//ลดจนถึงล่าสุด
    }
    //ถ้าไม่ใช่งานที่จะเตือนให้มันหาบรรทัดใหม่
  }
  //ถ้าเจอที่ใช่ก็จบๆกันไปเส่
}
