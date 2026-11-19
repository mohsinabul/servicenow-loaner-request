# Source review

Reviewed against the application XML in this repository on 19 November 2026. This is a static configuration review; the application was not executed or tested on a live instance during this review.

## Confirmed configuration

| Area | Evidence |
| --- | --- |
| Application | `Loaner Request`, scope `x_cdltd_loaner_r_0`. |
| Data | Loaner Request and Loaner Task table definitions, custom fields and form configuration. |
| Client logic | Active `Set Loaner Location` and `Set Loaner Item Type` scripts. |
| Validation | Active `Validate Start and End Date` rule aborts insert/update when the end date is earlier than the start date. Equal dates are not rejected by this comparison. |
| Scratchpad | Active `Set Scratchpad Values` display rule passes Requested for city/country to the client. |
| UI Policy | Active policy makes the Other depot field visible and mandatory under its configured condition. |
| Fulfilment | One active `Loaner Laptop` flow. Snapshot records are not counted as additional independent flows. |
| Reminders | Three active scheduled scripts call `LoanerUtils`; three active notifications respond to the corresponding events. |
| Reminder properties | Pickup lead time, return reminder window and overdue repeat interval are each configured as 24 hours. Job execution frequency is separate from these properties. |
| Security configuration | Two scoped roles and eleven ACL records. Live role permissions have not been verified here. |
| ATF | One active test definition named `My First Loaner Request ATF Test`. No `sys_atf_step` records or test-run results were found. |

## Laptop flow

The exported trigger selects laptop requests in state `14` (Reserved). The six configured actions are:

1. Wait for the request to reach state `15`.
2. Create the child task `Prepare the laptop for deployment` and wait for completion.
3. Update the request to state `16` (Ready for Pickup).
4. Wait for state `18`.
5. Create the child task `Perform the post deployment tasks.` and wait for completion.
6. Update the request to state `3` (Closed Complete).

The flow does not establish automatic inventory allocation or an approval process. Neither is claimed in the README.

## Lab examples that need attention

- **`Set Reserved State`:** this active Business Rule contains a placeholder script. Its name is not evidence that it sets the state automatically. Review the full rule configuration in the instance before relying on it.
- **`Retrieved Credentials`:** this inactive REST example contained a hard-coded lab password and logged the response/password. The local public-source copy has been sanitised. It remains inactive and is not presented as a working integration.
- **Notification mail-script example:** a second REST example also contained a hard-coded lab credential and printed a retrieved device password into email. Both the embedded credential and password output were removed. The example still requires secure configuration in the source instance; its integration behaviour is not claimed as verified.
- **ATF:** the exported test definition alone does not prove a complete or passing automated test. Configure and run the intended steps in the instance, then capture the results before claiming test coverage.

## Credential and export handling

Two REST example XML files were changed inside the generated application folder: the inactive Business Rule and the notification mail script. Their embedded password literals are now empty, and statements that logged or printed password-vault data were removed. The directory layout and other generated records were retained.

The original generated checksum was not recomputed. This sanitised portfolio snapshot should not be described as an unchanged, import-ready ServiceNow export. Make the cleanup in the source instance and export again if installation is needed.

The earlier GitHub history may still contain the original credential. If that credential remains valid, rotate or revoke it on the relevant lab instance. A new commit does not remove it from previous commits. No history rewrite or credential change was performed here.

## Useful next evidence

- A screenshot of the employee form and a resulting request.
- A completed laptop-flow execution and its child tasks.
- A reminder event and corresponding generated email record.
- A date-validation check with an invalid range.
- A completed ATF run, with its test steps visible.

These are proposed checks, not completed tests. Screenshots from other people's instances should remain learning references rather than evidence for this project.
