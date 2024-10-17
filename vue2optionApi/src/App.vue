<script>
import { defineComponent } from 'vue'
import Component from "@/views/HomeView.vue"
export default defineComponent({
  data() {
    return {
      name: "test",
      data: [
        {
          "transactionForSerialNumbers": [
            {
              "DocDate": "2023-11-12T00:00:00",
              "CardCode": "V00253",
              "CardName": "PMI LEBANON",
              "ObjType": 18,
              "DocEntry": 1120237,
              "LineNum": 0,
              "DocNum": 231104151,
              "EditedStatus": "1",
              "CANCELED": "N",
              "DocStatus": "O"
            },
            {
              "DocDate": "2023-11-12T00:00:00",
              "CardCode": "CSL00632",
              "CardName": "ABEER MALIK",
              "ObjType": 13,
              "DocEntry": 1011365,
              "LineNum": 1,
              "DocNum": 231094765,
              "EditedStatus": "1",
              "CANCELED": "N",
              "DocStatus": "O"
            }
          ],
          "ItemCode": "Ticket",
          "DocEntry": 1187479,
          "BaseLinNum": 0,
          "ManufacturerSerialNumber": "5GRGREG",
          "ExpiryDate": "0001-01-01T00:00:00",
          "ManufactureDate": "2023-11-12T00:00:00",
          "ReceptionDate": "2023-11-12T00:00:00",
          "Quantity": 1.0,
          "U_Surname": "hreg",
          "U_Airline": "HT",
          "U_DRouting": "HRT-HRT",
          "U_RRouting": "",
          "U_RBDClass": "",
          "U_CabinClass": "Economy Class",
          "U_PassangerType": "Adult",
          "U_GivenName": "greg",
          "U_FlightNumber": "",
          "U_TicketStatus": "Ticketed",
          "U_CTNM": "",
          "U_Journeytype": "INTERNATIONAL",
          "U_TicketSource": "GALILEO",
          "U_ItemDescription": 0,
          "U_SubItemDescription": 0
        },
        {
          "transactionForSerialNumbers": [
            {
              "DocDate": "2023-11-12T00:00:00",
              "CardCode": "V00253",
              "CardName": "PMI LEBANON",
              "ObjType": 18,
              "DocEntry": 1120237,
              "LineNum": 0,
              "DocNum": 231104151,
              "EditedStatus": "1",
              "CANCELED": "N",
              "DocStatus": "O"
            },
            {
              "DocDate": "2023-11-12T00:00:00",
              "CardCode": "CSL00632",
              "CardName": "ABEER MALIK",
              "ObjType": 13,
              "DocEntry": 1011365,
              "LineNum": 1,
              "DocNum": 231094765,
              "EditedStatus": "1",
              "CANCELED": "N",
              "DocStatus": "O"
            }
          ],
          "ItemCode": "Ticket",
          "DocEntry": 1187480,
          "BaseLinNum": 0,
          "ManufacturerSerialNumber": "43TGRGRE",
          "ExpiryDate": "0001-01-01T00:00:00",
          "ManufactureDate": "2023-11-12T00:00:00",
          "ReceptionDate": "2023-11-12T00:00:00",
          "Quantity": 1.0,
          "U_Surname": "greg",
          "U_Airline": "HT",
          "U_DRouting": "HRT-HRT",
          "U_RRouting": "",
          "U_RBDClass": "",
          "U_CabinClass": "Economy Class",
          "U_PassangerType": "Adult",
          "U_GivenName": "gegr",
          "U_FlightNumber": "",
          "U_TicketStatus": "Ticketed",
          "U_CTNM": "",
          "U_Journeytype": "INTERNATIONAL",
          "U_TicketSource": "GALILEO",
          "U_ItemDescription": 0,
          "U_SubItemDescription": 0
        },
        {
          "transactionForSerialNumbers": [
            {
              "DocDate": "2023-11-12T00:00:00",
              "CardCode": "V00253",
              "CardName": "PMI LEBANON",
              "ObjType": 18,
              "DocEntry": 1120237,
              "LineNum": 0,
              "DocNum": 231104151,
              "EditedStatus": "1",
              "CANCELED": "N",
              "DocStatus": "O"
            },
            {
              "DocDate": "2023-11-12T00:00:00",
              "CardCode": "CSL00632",
              "CardName": "ABEER MALIK",
              "ObjType": 13,
              "DocEntry": 1011365,
              "LineNum": 1,
              "DocNum": 231094765,
              "EditedStatus": "1",
              "CANCELED": "N",
              "DocStatus": "O"
            }
          ],
          "ItemCode": "Ticket",
          "DocEntry": 1187481,
          "BaseLinNum": 0,
          "ManufacturerSerialNumber": "43YTGRESDGREG",
          "ExpiryDate": "0001-01-01T00:00:00",
          "ManufactureDate": "2023-11-12T00:00:00",
          "ReceptionDate": "2023-11-12T00:00:00",
          "Quantity": 1.0,
          "U_Surname": "greg",
          "U_Airline": "HT",
          "U_DRouting": "HRT-HRT",
          "U_RRouting": "",
          "U_RBDClass": "",
          "U_CabinClass": "Economy Class",
          "U_PassangerType": "Adult",
          "U_GivenName": "gre",
          "U_FlightNumber": "",
          "U_TicketStatus": "Ticketed",
          "U_CTNM": "",
          "U_Journeytype": "INTERNATIONAL",
          "U_TicketSource": "GALILEO",
          "U_ItemDescription": 0,
          "U_SubItemDescription": 0
        }
      ]
    };
  },
  mounted() {
    this.name = "mounted"
  },
  components: {
    Component
  },
  method: {
    buttonSubmitted() {
      console.log("yesss")
    }
  }
});
</script>

<template>
  <Component @submitted="buttonSubmitted" />
  <div>
    {{ this.data[2]?.transactionForSerialNumbers[1]?.ObjType }}
  </div>
</template>
