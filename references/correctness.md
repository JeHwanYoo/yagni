# The part looks optional and is wrong, not smaller, if deferred

Two kinds of part need more care than the default "no present requirement, so defer" comparison. For both, an empty "needed now by" may be the wrong answer because requirements often omit correctness constraints.

## Parts whose absence is a defect today

Requirements describe the happy path, so these rarely appear in one. Nothing is deployed yet, so there is no observed failure. No constraint was stated. A superficial present-need check comes out empty — and the part ships missing.

- **Authorization and tenancy** on any path that reads or writes data belonging to someone other than the caller. `GET /orders?userId=` without an ownership check is an IDOR, and the ticket that asked for the endpoint did not mention ownership because tickets never do.
- **Validation at a trust boundary.** Input arriving from outside the process — a webhook body, a query string, a file — is checked before it is used, or the first malformed payload decides what your code does.
- **The backfill, constraint and rollback that make a schema change consistent.** A migration that adds the column but not the backfill leaves two meanings of null in production. A migration with no down path is a change that cannot be corrected.
- **Idempotency where a retry doubles an effect.** Payment capture, inventory decrement, point accrual, outbound email. Every delivery mechanism retries; the question is only whether the second delivery is harmless.
- **The audit record whose absence is the failure.** When the requirement is "we can show who changed this", the log is the feature, not instrumentation on it.

Material risk is their present justification. The test that separates them from real speculation: is the smaller version *less capable*, or *incorrect for a case that can happen today*? A webhook handler without a second provider is less capable. A webhook handler without signature verification is wrong.

From a run of this skill on a payment-webhook task, the agent classified these correctly on its own:

```text
결제 조회 API 로 본문 검증
  needed now by: 위험 — 본문에 서명이 없어 orderId 만 알면 결제 완료로 위조 가능
  if deferred, later cost: 미룰 수 없음. 미검증 수신은 지금 틀린 코드다

500 응답으로 재전송 유도 / 400 으로 재전송 차단
  needed now by: 위험 — 반영 중 DB 장애 시 200 을 주면 이벤트가 사라진다
  if deferred, later cost: 미룰 수 없음
```

"미룰 수 없음" in the later-cost field is the tell. When you find yourself writing "cannot defer", the part is in this section, and it ships complete — with its tests.

## Seams that are cheap to place and expensive to insert

The second kind has no direct present requirement and a later cost that reads "edit every existing call site". That retrofit cost is the argument for considering it now.

- A message catalog versus inline user-facing strings. Adding a second locale later means touching every string in every component.
- A currency- or timezone-carrying type versus a bare number or naive timestamp. Retrofitting means finding every arithmetic site.
- Tenant or account scoping in the data-access layer. Adding it later means auditing every query.
- A correlation id in the event envelope. Adding it later means every producer and every consumer.

For these, the comparison decides; an empty present-need field does not. Placing the seam now costs one decision; inserting it later costs one edit per call site, and the number of call sites only grows.

Fowler raises this exception himself and conditions it on experience. Internationalization is his example: if you have done it several times, you know which seam you need and placing it now is defensible; if you have not, you will probably get the seam wrong anyway, and the retrofit you are avoiding is the one that would have taught you where it goes. For a seam of this kind, also ask whether you have built it before; an honest "no" weakens the case for placing it now.

## What this section does not license

It is not a list of things to always add. It names cases that need a real risk assessment instead of a reflexive `nothing`. A webhook endpoint with no retry semantics is wrong; a webhook endpoint with a pluggable retry-policy strategy is speculative. The same comparison distinguishes them; this section only stops the first from being cut by mistake.
