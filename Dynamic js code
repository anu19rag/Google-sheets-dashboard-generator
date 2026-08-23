/**
 * 1. Automatic On Open
 */
function onOpen() {
  SpreadsheetApp.getUi()
    .createMenu('📊 Dashboard Manager')
    .addItem('🔄 Refresh Dashboard', 'createDashboard')
    .addToUi();
}

/**
 * 2. Main Dashboard Generator (Robust & Error-Free)
 */
function createDashboard() {
  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheets = ss.getSheets();
  var dashboardName = "Dashboard";
  var spreadsheetUrl = ss.getUrl();
  
  var dashboard = ss.getSheetByName(dashboardName);
  var savedExpectedValues = {};

  // Check if Dashboard exists;
  if (dashboard) {
    var prevLastRow = dashboard.getLastRow();
    if (prevLastRow > 1) {
      try {
        var prevData = dashboard.getRange(2, 2, prevLastRow - 1, 3).getValues();
        for (var k = 0; k < prevData.length; k++) {
          var sName = prevData[k][0];
          var expVal = prevData[k][2];
          if (sName && expVal !== "") {
            savedExpectedValues[sName] = expVal;
          }
        }
      } catch (e) {
        Logger.log("Preserve error: " + e.message);
      }
    }
    dashboard.clear();
    dashboard.activate();
  } else {
    dashboard = ss.insertSheet(dashboardName, 0);
  }

  // Ensure Dashboard is the first tab
  try {
    ss.setActiveSheet(dashboard);
    ss.moveActiveSheet(1);
  } catch (e) {
    // Ignore if already at position 1
  }

  var headers = [[
    "S.No.", 
    "Sheet / Tab Name", 
    "Quick Navigation", 
    "Expected Rows", 
    "Actual Data Rows", 
    "Verification Status", 
    "Last Checked"
  ]];
  
  var rowsData = [];
  var statusColors = [];
  var bgColors = [];
  var serialNo = 1;
  var timestamp = Utilities.formatDate(new Date(), ss.getSpreadsheetTimeZone() || Session.getScriptTimeZone(), "dd-MMM-yyyy HH:mm");

  for (var i = 0; i < sheets.length; i++) {
    var sheet = sheets[i];
    var sheetName = sheet.getName();

    if (sheetName === dashboardName) continue;

    var sheetId = sheet.getSheetId();
    var sheetUrl = spreadsheetUrl + "#gid=" + sheetId;
    
    // Non-header rows count
    var lastRow = sheet.getLastRow();
    var actualDataRows = lastRow > 1 ? (lastRow - 1) : 0;
    
    var expVal = savedExpectedValues.hasOwnProperty(sheetName) ? savedExpectedValues[sheetName] : "";
    
    var status = "";
    var statusBg = "#FFFFFF";
    var statusText = "#000000";

    if (expVal === "" || isNaN(expVal)) {
      status = "⚠️ Enter Target";
      statusBg = "#FEF3C7";
      statusText = "#92400E";
    } else {
      var expNum = Number(expVal);
      if (expNum === actualDataRows) {
        status = "✅ MATCH (" + actualDataRows + ")";
        statusBg = "#D1FAE5";
        statusText = "#065F46";
      } else {
        var diff = actualDataRows - expNum;
        var diffText = diff > 0 ? ("+" + diff + " extra") : (diff + " missing");
        status = "❌ MISMATCH (" + diffText + ")";
        statusBg = "#FEE2E2";
        statusText = "#991B1B";
      }
    }

    var hyperlinkFormula = '=HYPERLINK("' + sheetUrl + '", "Open ➜ ' + sheetName.replace(/"/g, '""') + '")';

    rowsData.push([
      serialNo,
      sheetName,
      hyperlinkFormula,
      expVal,
      actualDataRows,
      status,
      timestamp
    ]);

    statusColors.push([statusBg, statusText]);
    var rowBg = (serialNo % 2 === 0) ? "#F8FAFC" : "#FFFFFF";
    bgColors.push([rowBg, rowBg, rowBg, "#F1F5F9", rowBg, statusBg, rowBg]);

    serialNo++;
  }

  var totalItems = rowsData.length;
  if (totalItems > 0) {
    // 1. Headers Styling
    var headerRange = dashboard.getRange(1, 1, 1, 7);
    headerRange.setValues(headers);
    headerRange.setBackground("#1E293B");
    headerRange.setFontColor("#FFFFFF");
    headerRange.setFontWeight("bold");
    headerRange.setFontSize(10);
    headerRange.setHorizontalAlignment("center");
    headerRange.setVerticalAlignment("middle");
    dashboard.setRowHeight(1, 38);

    // 2. Data Population
    var dataRange = dashboard.getRange(2, 1, totalItems, 7);
    dataRange.setValues(rowsData);
    dataRange.setBackgrounds(bgColors);
    dataRange.setFontSize(10);
    dataRange.setVerticalAlignment("middle");

    // 3. Alignments
    dashboard.getRange(2, 1, totalItems, 1).setHorizontalAlignment("center");
    dashboard.getRange(2, 2, totalItems, 1).setHorizontalAlignment("left");
    dashboard.getRange(2, 3, totalItems, 1).setHorizontalAlignment("center");
    dashboard.getRange(2, 4, totalItems, 1).setHorizontalAlignment("center").setFontWeight("bold");
    dashboard.getRange(2, 5, totalItems, 1).setHorizontalAlignment("center").setFontWeight("bold");
    dashboard.getRange(2, 6, totalItems, 1).setHorizontalAlignment("center").setFontWeight("bold");
    dashboard.getRange(2, 7, totalItems, 1).setHorizontalAlignment("center").setFontColor("#64748B");

    // 4. Status Font Colors
    for (var r = 0; r < statusColors.length; r++) {
      dashboard.getRange(r + 2, 6).setFontColor(statusColors[r][1]);
    }

    // 5. Grid Borders
    dashboard.getRange(1, 1, totalItems + 1, 7).setBorder(
      true, true, true, true, true, true, 
      "#CBD5E1", 
      SpreadsheetApp.BorderStyle.SOLID
    );

    for (var rowIdx = 2; rowIdx <= totalItems + 1; rowIdx++) {
      dashboard.setRowHeight(rowIdx, 28);
    }
  }

  dashboard.autoResizeColumns(1, 7);
  for (var col = 1; col <= 7; col++) {
    dashboard.setColumnWidth(col, dashboard.getColumnWidth(col) + 20);
  }
}
