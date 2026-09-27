# Submission contents

This folder contains the current local project artifacts and the supplied
screenshots. See OBSERVATIONS.md for results and known limitations.

| Requirement | Evidence |
| --- | --- |
| Completed system prompt | system_prompt.txt; online_shop_faq.md is included because the harness setup expands the FAQ placeholder |
| Harness and gateway ARNs | agentcore_config.json |
| Bug-report transcript with follow-ups and tool call | chat_transcripts.txt, section 1; terminal screenshot dated September 27 |
| DynamoDB screenshot showing chatbot-created items | screenshots/screencapture-us-east-1-console-aws-amazon-dynamodbv2-home-2026-09-25-00_24_04.png |
| Covered question transcript | chat_transcripts.txt, section 1 |
| Uncovered shop question transcript | chat_transcripts.txt, section 3; screenshots/Screenshot From 2026-09-28 00-46-15.png |
| Out-of-scope request transcript | chat_transcripts.txt, section 2; recorded with an earlier prompt version |
| Test cases | harness-tests.json |
| Generated evaluation dataset | output_eval_dataset.jsonl |
| Written observations | OBSERVATIONS.md |
| Evaluation results screenshot | screenshots/screencapture-us-east-1-console-aws-amazon-bedrock-home-2026-09-25-00_15_32.png |

The September 28 terminal screenshot at 00-38-36 shows saved ticket IDs, including the
September 27 ticket in the bug-report transcript. The S3 screenshot provides
additional dataset-upload evidence. The CloudWatch screenshot shows an AgentCore
evaluation log group; use the Bedrock results screenshot for the scored model
evaluation.

The evaluation screenshot and dataset belong to the earlier evaluated version.
The included system prompt and chat client contain later revisions. Their
remaining limitations and this version difference are stated in OBSERVATIONS.md.

All requested file and evidence categories are now represented. The gift-wrapping
conversation supplies the uncovered-question evidence. This checklist confirms
the presence of the deliverables, not that every tested behavior met the rubric;
see OBSERVATIONS.md for the remaining limitations.
