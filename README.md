import moment from "moment";
import { RowNode } from "ag-grid-community";

gridRef.current.api.forEachNode((node: RowNode) => {
  if (node.data && node.data.eventTimeStamp) {
    // Format the date to YYYYMMDD
    const formattedDate = moment(node.data.eventTimeStamp, ["M/D/YYYY h:mm:ss A", "YYYYMMDD"], true)
      .format("YYYYMMDD");

    // Update the row data
    node.setDataValue("eventTimeStamp", formattedDate);

    console.log("Updated Row:", node.data);
  }
});
