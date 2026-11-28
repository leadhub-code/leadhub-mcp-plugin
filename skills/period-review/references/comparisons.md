# Period comparisons

The analytics tools return one range. They do not return a delta. The web computes two comparisons
by calling the same endpoint again:

- Previous period: the same length, ending when the current range starts.
  If the range is from_date .. to_date and lasts D, the previous range is (from_date - D) .. from_date.
- Year ago: the same calendar dates shifted back one year.

Call the same tool with those dates. A rate's change is in percentage points (subtract the two rates).
A count's change is a relative percent: (current - previous) / previous.
