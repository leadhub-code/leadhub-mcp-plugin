# RFM

A profile stores window_days, recency/frequency/monetary bounds, and named segments.
Each segment lists score strings such as "544": recency bucket, frequency bucket, monetary bucket.

`get_rfm_analysis` classifies customers in one window ending at reference_date
(from_date = reference_date - window_days) and rolls buckets into those names.
Buckets the profile does not name come back as "(unassigned)".

`rfm_segment_composition` does that roll at each reference date you pass.
The page uses the last 12 month-ends.

`rfm_movement` compares two reference dates.
segment_movements are named, and triples the profile does not name are "(unassigned)".
"(not in earlier window)" is where a customer comes from who was not in the earlier window.
"(not in later window)" is where a customer goes who was not in the later one.
They are in parentheses so they do not mix with a profile segment such as "Churned".
bucket_movement is the headcount change of each recency, frequency, and monetary bucket.
delta is the later window minus the earlier one.

These tools do not edit profiles.
