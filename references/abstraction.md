# The part is an abstraction for a second thing that is planned, not present

An interface with one implementation. A registry with one entry. A plugin seam nothing plugs into. A config flag with one value. A `--provider` option when there is one provider. This is the case YAGNI was named for, and the one where the argument for building now sounds most reasonable, because the second thing usually is coming.

## Why the argument fails even when the second thing comes

Fowler, on exactly this case: you may not need the other variants, and if you do, your current idea of what the abstraction should look like will not match what you learn when you actually build the second one. The interface you extract from one implementation is shaped by that implementation. The second one arrives with a difference you did not predict — a different auth scheme, a synchronous refund where the first was asynchronous — and the interface either grows a parameter for it or gets rewritten. Either way you paid carry on it for a quarter and then paid repair.

The smell has a name in the refactoring catalogue: Speculative Generality.

## The comparison, worked

Asked for: accept order webhooks from one payment provider. A second provider is on the roadmap for next quarter.

```text
signature verifier
  needed now by: requirement — the provider signs every payload, and unsigned
                 intake would pass silently
  if deferred, later cost: cannot defer, unsigned intake is wrong today

provider interface + handler registry
  needed now by: nothing — the second provider is planned, not present
  if deferred, later cost: extract an interface from two concrete handlers,
                           mechanical, under an hour
```

The registry goes, even though the second provider is genuinely coming. A planned need is not a present one, and an hour of mechanical extraction later beats a quarter of carry now. The verifier stays and ships complete with its failing-signature test.

Now change one fact. The second provider is contracted, its credentials are already in the repo, and the two disagree on whether refunds settle synchronously. "Needed now by" is now a project constraint; the interface models a difference you have seen rather than one you imagine; you build it for those two cases only. Same part, opposite answers. The evidence separates them.

## The same case, as an agent actually recorded it

From a run of this skill on the Korean version of that task (Toss Payments now, Kakao Pay next quarter). The records were written in the reply, in the task's language:

```text
결제사 공통 인터페이스 (PaymentProviderWebhook 등) + 결제사별 핸들러 레지스트리
  needed now by: nothing — 카카오페이는 계획이지 현재 코드에 없다. 카카오페이 웹훅의
                 검증 방식과 이벤트 형태를 아직 모르므로 인터페이스가 무엇을 공통으로
                 가져야 하는지도 알 수 없다
  if deferred, later cost: 카카오페이 라우터를 따로 만든 뒤, 두 구현을 보고 공통부가 있으면
                           그때 추출. toss.ts 는 100줄이고 호출부는 app.ts 한 곳 → 기계적,
                           한 시간 안쪽
  → 제거

/webhooks/:provider 식 동적 라우팅
  needed now by: nothing
  if deferred, later cost: 카카오페이 추가 시 app.use 한 줄
  → 제거
```

The second record matters. The agent did not stop at rejecting the interface; it also caught the routing convention that was the interface in another form.

## The disguised version

The same run without this skill also rejected the `PaymentProvider` interface — and then kept three "provider-neutral" scaffolds: a `/webhooks/{provider}` path convention so the next provider "fits naturally", a `toss:` prefix on storage keys so the next provider "can share the store", and a body-parser mounting order justified by "when we add a provider that needs signature verification". Each was defended with the same future feature the interface had just been rejected for.

This is the common shape of the failure. The named abstraction gets cut because it is obvious; its supporting scaffolding survives because each piece is small and each has a story. Apply the same comparison to material scaffolding: in this case each piece has the same `needed now by: nothing` answer.

## Options and flags

A configuration option with one value in use is this case with different syntax. Ask who sets the other value, today. If the answer is "someone might", the option is a presumptive feature with a switch on it. Hard-code the value; the option is a two-line change when a second value exists.

The exception, from `costs.md`: a move that only changes where a value lives — a constant instead of a literal, a table instead of inline strings — adds no capability and needs no separate comparison. A flag adds a capability: two behaviours where there was one.

## When the answer is build

Present justifications for this case include:

- two implementations exist in the codebase today, and the extraction makes both clearer
- a contract, credentials, or a test fixture for the second thing is already in the repo — the difference is observed, not imagined
- the seam is one of the retrofit-hostile kind in `correctness.md`, where the later-cost field reads "edit every call site" and the comparison, not the empty field, decides

Build it for the cases in front of you, without growing beyond the justification that kept it.
