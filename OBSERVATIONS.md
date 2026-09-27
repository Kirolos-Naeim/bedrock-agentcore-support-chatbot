# Project Observations: Customer Support Chatbot

## Purpose and approach

I built a customer-support chatbot using an Amazon Bedrock AgentCore managed harness. The system prompt defines three routes: answer shop questions from the supplied FAQ, collect website bug details and submit a report through the AgentCore Gateway, or refer unsupported requests to human support. The bug-report tool uses Lambda to store reports in DynamoDB.

## What worked

The chatbot answered the delivery-time question using the FAQ. It explained that delivery estimates appear at checkout and in the shipping confirmation email, and that processing normally takes 1–2 business days before dispatch. This distinguishes processing time from the delivery estimate instead of promising an unsupported arrival date.

In successful bug-report conversations, the chatbot collected a description, reproduction steps, and environment across multiple turns. A later test without verbose logging returned ticket ID `6c28d528-7b9e-4895-8575-306aab55d089`. A strongly consistent DynamoDB lookup confirmed that this item existed with the checkout-crash description, reproduction steps, environment `linux,fedora`, and status `OPEN`. This provides evidence that the application can submit and persist a report.

## Evaluation results

The completed evaluation used three generated responses: a delivery FAQ question, a complete bug report containing all required fields, and an out-of-scope movie request.

| Metric | Aggregate score |
| --- | ---: |
| Correctness | 1.00 |
| Helpfulness | 0.78 |
| Following instructions | 0.67 |

Correctness was the strongest result, while following instructions was the weakest. These are aggregate scores for a small dataset, not percentages of all possible conversations that will succeed. I did not use per-example evaluator explanations to attribute the lower scores to a particular response.

The evaluation was completed before the later prompt and chat-client revisions. These scores describe that earlier version; they do not measure the final revised prompt. In addition, the complete bug-report evaluation input did not test the multi-turn collection behavior that caused problems in manual testing.

## Problems found during manual testing

Some replies claimed a ticket had been created without a corresponding tool-call marker. For example, the chatbot returned `BUG-1234`, but a DynamoDB lookup found no matching item. This showed why a fluent success message is insufficient evidence of a completed action.

Other replies reused earlier ticket IDs or referred to an issue as already reported. A separate actor ID was added for each chat to isolate its memory, but the observations alone do not establish that memory was the only cause of incorrect replies.

The latest verified submission still had two problems: the chatbot invoked the tool before the environment was supplied, and it displayed `<thinking>` text despite the prompt's response rules. The result of that first, premature invocation was not captured in the shared transcript, so I cannot say whether it failed validation or created another item.

An earlier movie-request response recognized the topic as outside shop support but suggested movie websites instead of consistently using the required human-support handoff. A separate shop-related question not covered by the FAQ has not yet been documented in the available transcripts.

## Changes and lessons learned

I revised the prompt to give an explicit sequence: ask for one missing field, invoke `bugreports___create_bug_report` once all three fields are available, and confirm creation only from a successful tool result containing a ticket ID. The prompt also clarifies how to handle short replies, repeated steps, meaningful environment details, and separate issues.

A client-side safeguard now replaces some ticket-success claims when no bug-report tool call was observed in that turn. It is a phrase-based check, so it is not a complete guarantee. Observing a tool call also does not prove that the tool succeeded or that the assistant repeated the returned ID accurately.

Verbose logging helped inspect actual tool requests and successful results. An offline replay of the same successful events produced the same reply and tool-call marker in both normal and verbose modes. This supports using verbose mode for diagnosis rather than treating it as a fix for inconsistent tool use.

The main lesson is to verify agent actions using tool results and stored records, in addition to evaluating the wording of responses. Further improvements should validate confirmations against the matching successful tool result, check required fields in the tool itself, and expand evaluation to missing fields, spelling variations, repeated messages, uncovered FAQ questions, tool failures, and multiple issues in one conversation. The revised version should then be evaluated again.
