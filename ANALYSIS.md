# Vector and Scalar Evaluation

## Tracing the Missing Value
We used the sample c(Ada = 8, Grace = NA, Linus = 12, Barbara = 4). Both functions return c(Ada = 16, Grace = 0, Linus = 20, Barbara = 8), but they get there differently.

In clean_scores_vector(), each line works on the whole vector at once:

1. out[is.na(out)] <- 0, finds every NA and sets it to 0, so Grace becomes 0.
2. out <- out * 2, doubles every score, so Ada becomes 16, Grace stays 0, Linus becomes 24, and Barbara becomes 8.
3. out[out > 20] <- 20, caps every score above 20. Ada, Grace, and Barbara’s values are not above 20, so they stay as is, while Linus’s 24 becomes 20.

In clean_scores_scalar(), the for loop visits one student per pass. Grace is second, so on pass 2 value <- scores[[i]] gives NA. The check is.na(value) is TRUE, so the loop sets the value to 0, skips the doubling, and saves it with out[[i]] <- value. The other passes double their value and cap it at 20 if needed.

## Operations on All Elements

In the vector version, three operations change every element in one step: the is.na() check with its replacement by 0, the out * 2 doubling, and the out > 20 check that sets its value to 20.

## Where the Order Shows

The order is visible at for (i in seq_along(scores)). The loop handles Ada, then Grace, then Linus, then Barbara. Inside it, the if (is.na(value)) and if (value > 20) checks run once per student, so you can stop at any pass and see which student is being cleaned.

## A Recycling Mistake

If someone wrote out * c(2, 1) instead of out * 2, R would repeat c(2, 1) across the four scores. Ada and Linus would be doubled, but Grace and Barbara would be multiplied by 1. Barbara would get 4 instead of 8. R gives no warning because four divides evenly by two.

## Which One We Would Keep
We would keep clean_scores_vector(). It is shorter and faster on large data because R does the looping for us. The tradeoff for that is that we cannot see the order of the work. If something goes wrong, the scalar loop is easier to debug because we can follow it one student at a time.
