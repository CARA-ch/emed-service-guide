# PADV document

## Header

## Sections

## Item entry

It's an `#!xml <hl7:observation>`.

### Type


### Text

In PADV COMMENT: it contains the actual comment.
In PADV OK, SUSPEND, CANCEL, REFUSE, CHANGE: it contains the narrative representation of the item entry.

### Status code

| Code        | Description |
| ----------- | ----------- |
| `active`    | The entry is a provisional result, it's considered as a pre-release advice and does not affect the workflow (i.e. the eMedication service does not aggregate it). |
| `completed` | The entry is a final result. It's aggregated by the eMedication service. |

Use `completed` unless you've got a real use-case for `active` (e.g. sharing PADV drafts between healthcare professionals).

```xml title="Usage example"
<hl7:statusCode value="completed" />
```

### Item effective time

The date at which the entry becomes effective (optional); if absent, the item becomes effective at the document creation time (`#!xquery hl7:ClinicalDocument/hl7:effectiveTime`).

!!! warning "CARA: additional requirement"

    The eMedication service restricts the entry effective time to the past and present; it shall not be in the future.

```xml title="Usage example"
<hl7:effectiveTime value="20220123123000+0100" />
```

### Annotation comment

In PADV OK, SUSPEND, CANCEL, REFUSE, CHANGE: it is used to express the reason why the action was done.
There's no use-case for PADV COMMENTs.
