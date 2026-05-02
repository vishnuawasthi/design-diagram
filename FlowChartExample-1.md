``````mermaid
flowchart LR
  classDef system fill:#1f2937,stroke:#9ca3af,stroke-width:1px,color:#f8fafc,font-weight:600
  classDef topic fill:#0f766e,stroke:#2dd4bf,stroke-width:1px,color:#ecfeff,font-weight:600
  classDef service fill:#2563eb,stroke:#93c5fd,stroke-width:1px,color:#eff6ff,font-weight:600
  classDef action fill:#d97706,stroke:#fbbf24,stroke-width:1px,color:#fff7ed,font-weight:600

  UI[XVA TOP (UI)]:::system
  XVARequest[XVARequest / XVARequestData<br/>Relayer AMPS topics]:::topic
  ReportRunner[ReportRunner]:::service
  RRToSR[RRToSR<br/>Base pricing request topic]:::topic
  EngineToService[EngineToServiceRequest<br/>KVA processing queue]:::topic
  ScenarioRunner[Scenario Runner]:::service
  SRToRRAck[SRToRRAck<br/>Scenario ack topic]:::topic
  IBMGrid[IBM Symphony Grid]:::service
  MinimumResult[MinimumResult<br/>Relayer AMPS topic]:::topic
  KVAService[KVAService]:::service
  KVA_BFL[KVA_BFL<br/>Relayer AMPS topic]:::topic

  UI -->|Send request with KVA/SAccr action| XVARequest
  XVARequest -->|Picked by report runner| ReportRunner
  ReportRunner -->|Process snapshot and actions| ReportRunner
  ReportRunner -->|Submit Base pricing task| RRToSR
  ReportRunner -->|Submit KVA task| EngineToService
  EngineToService -->|Consumed by scenario runners| ScenarioRunner
  ScenarioRunner -->|Acknowledge task| SRToRRAck
  ScenarioRunner -->|Submit task to IBM Symphony Grid| IBMGrid
  IBMGrid -->|Wait and publish results| MinimumResult
  MinimumResult -->|Picked by KVAService for Mars invocation| KVAService
  KVAService -->|Perform Mars invocation| KVAService
  KVAService -->|Save Mars response| KVA_BFL
  KVAService -->|Submit KVA request to IBM Symphony Grid| IBMGrid
  IBMGrid -->|Return response| MinimumResult

  class UI,XVARequest,ReportRunner,RRToSR,EngineToService,ScenarioRunner,SRToRRAck,IBMGrid,MinimumResult,KVAService,KVA_BFL system,topic,service,topic,topic,service,topic,service,topic,service,topic
```

```mermaid
flowchart TD
  classDef service fill:#1f2937,stroke:#9ca3af,stroke-width:1px,color:#f8fafc,font-weight:600
  classDef topic fill:#0f766e,stroke:#2dd4bf,stroke-width:1px,color:#ecfeff,font-weight:600
  classDef process fill:#2563eb,stroke:#93c5fd,stroke-width:1px,color:#eff6ff,font-weight:600

  UI[XVA TOP (UI)]:::service
  XVARequest[XVARequest / XVARequestData]:::topic
  ReportRunner[ReportRunner]:::process
  RRToSR[RRToSR (Base)]:::topic
  EngineToService[EngineToServiceRequest (KVA)]:::topic
  ScenarioRunner[Scenario Runner]:::process
  SRToRRAck[SRToRRAck]:::topic
  IBMGrid[IBM Symphony Grid]:::service
  MinimumResult[MinimumResult]:::topic
  KVAService[KVAService]:::process
  KVA_BFL[KVA_BFL]:::topic

  UI -->|1. Send request with KVA/SAccr action| XVARequest
  XVARequest -->|2. ReportRunner reads request| ReportRunner
  ReportRunner -->|3. Submit Base pricing task| RRToSR
  ReportRunner -->|4. Submit KVA task| EngineToService
  EngineToService -->|5. Scenario Runner consumes task| ScenarioRunner
  ScenarioRunner -->|6. Acknowledge task| SRToRRAck
  ScenarioRunner -->|7. Submit to IBM Symphony Grid| IBMGrid
  IBMGrid -->|8. Publish grid response| MinimumResult
  MinimumResult -->|9. KVAService picks request| KVAService
  KVAService -->|10. Perform Mars invocation| KVAService
  KVAService -->|11. Save Mars results| KVA_BFL
  KVAService -->|12. Submit KVA request to IBM Symphony Grid| IBMGrid
  IBMGrid -->|13. Publish final results| MinimumResult
```
