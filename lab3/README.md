# Lab 3

Instructions for this section will be provided in class and on Blackboard when we reach it.

Put your work for Lab 3 in this folder.

"Why a real account would use OIDC rather than stored keys?"

- Real account would use OIDC to obtain temporary AWS credentials instead of storing
long lived access keys in the repo - this reduces risk of credential exposure that
would occur from those long lived keys

"why this course uses session-scoped secrets and what limits the damage if they leak."
- Course uses session-scoped secrets because AWS Academy was said to restrict
IAM changes required for OIDC, the damage is limited even if leaked because the
session-scoped secrets is only 4 hours from the start of pressing Start Lab in
AWS Academy
