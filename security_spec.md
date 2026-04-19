# Security Specification - CampusConfessions

## Data Invariants
1. A review must belong to a valid college ID.
2. An anonymous user can only create reviews. 
3. Reviews once posted cannot be modified by the author (unless we add edit later, but requirement says "anonymous confessions", usually immutable).
4. Ratings must be between 0 and 10.
5. Headlines and content cannot exceed size limits (200 and 1000 chars).
6. Votes and reactions can be updated by anyone but within strict increments.

## The Dirty Dozen Payloads (Rejection Tests)
1. **Identity Spoofing**: Attempting to create a review with a fake `userId` not matching `request.auth.uid`.
2. **Path Variable Poisoning**: Using a 2KB string as a college document ID.
3. **Shadow Update**: Adding `isVerified: true` to a review.
4. **State Shortcutting**: Updating a review status from `pending` to `approved` as a regular user.
5. **Type Poisoning**: Sending a string for a rating field.
6. **Boundary Violation**: Sending a rating of 11.
7. **Size Bomb**: Sending a review content with 5MB of text.
8. **Relational Sync Break**: Creating a review for a non-existent college.
9. **Email Spoofing**: (N/A as we use anonymous auth mainly, but if we add admin by email, we test unverified emails).
10. **Reaction Spam**: Incrementing reactions by 100 in one update.
11. **PII Leak**: Reading a private user info collection (if we had one).
12. **Blanket Read Scam**: Trying to list all reviews without any filters if rules are intended to be restrictive.

## Test Runner
```typescript
// firestore.rules.test.ts
// (Implementation details below)
```
