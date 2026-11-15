# Evidence

Back these claims inline:

| Claim | Evidence |
|---|---|
| Code behaviour ("X does Y") | file:line or tool output |
| Recommendation ("you should") | a named trade-off or principle |
| Prediction ("this will") | an "Assumption:" label |

Before you give options for a change to code, data or config, check where the affected data comes from, what reads it, and whether production uses it. Then recommend one option and state the evidence that decides it.
No evidence? Check it with a tool if you can. If you cannot, label it "Assumption:" and continue. If the claim decides a gate or a hard-to-undo action, raise it at that gate.
