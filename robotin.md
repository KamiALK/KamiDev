---
layout: default
---

## Introduction

My curiosity led me to explore the scope of programming, and I realized it can solve very everyday problems. A colleague who owned a restaurant struggled with displaying orders to customers, which prompted me to research how to do it. I credit the person I saw doing it, Leifer Méndez, who created a library. I simply added the logic to make it work.

![demo](./images/robotino/Screen_Recording_20240503_191426_WhatsApp.gif)

## considerations

The basis of this project is ease of use, for which it is linked to a Google spreadsheet. This requires having an account and being able to use the Google Cloud Console.

![console](./images/robotino/console.png)

In addition to being able to use the console, you need to have the Google Sheets API enabled and configured in your credentials.

![API](./images/robotino/api.png)

In addition, you need to configure the credentials in service account mode so that the project can connect and have read and write permissions to the spreadsheet.

![credentials](./images/robotino/auth.png)

You also need to obtain the spreadsheet ID and add the service account's email address as an editor and reader of the spreadsheet.

![spreadsheet](./images/robotino/env.png)

## Sheet Connection

```javascript

class GoogleSheetService {
  jwtFromEnv = undefined;
  doc = undefined;

  constructor(id = undefined) {
    if (!id) {
      throw new Error("ID_UNDEFINED");
    }

    this.jwtFromEnv = new JWT({
      email: process.env.GOOGLE_SERVICE_ACCOUNT_EMAIL,
      key: process.env.GOOGLE_PRIVATE_KEY.replace(/\\n/g, "\n"),
      scopes: SCOPES,
    });
    this.doc = new GoogleSpreadsheet(id, this.jwtFromEnv);
  }
```

## read

A flow that captures information from the cells.

```javascript
retriveDayMenu = async (columnNumber = 0) => {
  try {
    const list = [];
    await this.doc.loadInfo();
    const sheet = this.doc.sheetsByIndex[0]; // the first sheet
    await sheet.loadCells("A1:J10");
    const rows = await sheet.getRows();
    for (const a of Array.from(Array(rows.length).keys())) {
      const cellA1 = sheet.getCell(a + 1, columnNumber - 1);
      list.push(cellA1.value || null);
    }

    return list;
  } catch (err) {
    console.log(err);
    return undefined;
  }
};
```

## write

escribir en las hojas de calculo la informacion obtenida del usuario

```javascript
saveOrder = async (data = {}) => {
  await this.doc.loadInfo();
  const sheet = this.doc.sheetsByIndex[1]; // the first sheet

  const order = await sheet.addRow({
    fecha: data.fecha,
    telefono: data.telefono,
    nombre: data.nombre,
    pedido: data.pedido,
    observaciones: data.observaciones,
  });

  return order;
};
```
