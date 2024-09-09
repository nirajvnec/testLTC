syntax = "proto3";

option csharp_namespace = "Mars.Proto.RiskServer";

package mars.proto.risk_server;

service BridgeG2C {
  rpc aggregatedVaR(CalcInputRepriceables) returns (CalcOutputVaRNumber) {}
  rpc exchangeRate(req_exchangeRate) returns (resp_exchangeRate) {}
}

message req_exchangeRate {
  string numerator_ccy = 1;
  string denominator_ccy = 2;
  VaRContext ctx = 3;
}

message resp_exchangeRate {
  VaRNumber out = 1;
}

message VaRNumber {
  double val = 1;
}

message VaRContext {
  repeated KeyValuePair sequence = 1;
}

message KeyValuePair {
  string key = 1;
  string value = 2;
}

// You need to define CalcInputRepriceables and CalcOutputVaRNumber
message CalcInputRepriceables {
  // Define fields here
}

message CalcOutputVaRNumber {
  // Define fields here
}
