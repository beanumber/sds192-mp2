sds192-mp2
================

Mini-project 2:

See (<https://beanumber.github.io/sds192/mod_data.html>) for the project
instructions

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.3.0 ──

    ## ✓ ggplot2 3.3.2     ✓ purrr   0.3.4
    ## ✓ tibble  3.0.3     ✓ dplyr   1.0.2
    ## ✓ tidyr   1.1.2     ✓ stringr 1.4.0
    ## ✓ readr   1.4.0     ✓ forcats 0.5.0

    ## ── Conflicts ────────────────────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
# install.packages("fec16")
library(fec16)
```

Verify that your data looks like this:

``` r
library(tidyverse)
glimpse(results_house)
```

    ## Rows: 2,110
    ## Columns: 13
    ## $ state           <chr> "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL", "AL",…
    ## $ district_id     <chr> "01", "01", "02", "02", "02", "02", "03", "03", "03",…
    ## $ cand_id         <chr> "H4AL01123", "H6AL01060", "H0AL02087", "H6AL02142", "…
    ## $ incumbent       <lgl> TRUE, FALSE, TRUE, FALSE, FALSE, FALSE, TRUE, FALSE, …
    ## $ party           <chr> "R", "R", "R", "R", "R", "D", "R", "R", "D", "R", "R"…
    ## $ primary_votes   <dbl> 71310, 47319, 78689, 33015, 6856, NA, 77432, 24474, N…
    ## $ primary_percent <dbl> 0.60111777, 0.39888223, 0.66370614, 0.27846660, 0.057…
    ## $ runoff_votes    <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    ## $ runoff_percent  <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…
    ## $ general_votes   <dbl> 208083, NA, 134886, NA, NA, 112089, 192164, NA, 94549…
    ## $ general_percent <dbl> 0.96382467, NA, 0.48768548, NA, NA, 0.40526205, 0.669…
    ## $ won             <lgl> TRUE, FALSE, TRUE, FALSE, FALSE, FALSE, TRUE, FALSE, …
    ## $ footnotes       <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N…

``` r
glimpse(candidates)
```

    ## Rows: 4,699
    ## Columns: 15
    ## $ cand_id              <chr> "H0AL02087", "H0AL02095", "H0AL05163", "H0AL0708…
    ## $ cand_name            <chr> "ROBY, MARTHA", "JOHN, ROBERT E JR", "BROOKS, MO…
    ## $ cand_pty_affiliation <chr> "REP", "IND", "REP", "DEM", "REP", "REP", "REP",…
    ## $ cand_election_yr     <dbl> 2016, 2016, 2016, 2016, 2016, 2016, 2016, 2016, …
    ## $ cand_office_st       <chr> "AL", "AL", "AL", "AL", "AR", "AR", "AZ", "CA", …
    ## $ cand_office          <chr> "H", "H", "H", "H", "H", "H", "H", "H", "H", "H"…
    ## $ cand_office_district <chr> "02", "02", "05", "07", "01", "03", "04", "07", …
    ## $ cand_ici             <chr> "I", "C", "I", "I", "I", "I", "I", "I", "I", "I"…
    ## $ cand_status          <chr> "C", "N", "C", "C", "C", "C", "P", "C", "C", "C"…
    ## $ cand_pcc             <chr> "C00462143", NA, "C00464149", "C00458976", "C004…
    ## $ cand_st1             <chr> "PO BOX 195", "1465 W OVERBROOK RD", "7610 FOXFI…
    ## $ cand_st2             <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, "123…
    ## $ cand_city            <chr> "MONTGOMERY", "MILLBROOK", "HUNTSVILLE", "BIRMIN…
    ## $ cand_st              <chr> "AL", "AL", "AL", "AL", "AR", "AR", "AZ", "CA", …
    ## $ cand_zip             <chr> "36101", "36054", "35802", "35201", "72404", "72…

``` r
glimpse(committees)
```

    ## Rows: 17,654
    ## Columns: 15
    ## $ cmte_id              <chr> "C00000059", "C00000422", "C00000489", "C0000054…
    ## $ cmte_nm              <chr> "HALLMARK CARDS PAC", "AMERICAN MEDICAL ASSOCIAT…
    ## $ tres_nm              <chr> "ERIN BROWER", "WALKER, KEVIN", "TOM RITTER", "C…
    ## $ cmte_st1             <chr> "2501 MCGEE", "25 MASSACHUSETTS AVE, NW", "3528 …
    ## $ cmte_st2             <chr> "MD#288", "SUITE 600", NA, NA, NA, "SUITE 1100",…
    ## $ cmte_city            <chr> "KANSAS CITY", "WASHINGTON", "OKLAHOMA CITY", "T…
    ## $ cmte_st              <chr> "MO", "DC", "OK", "KS", "IN", "DC", "MD", "DC", …
    ## $ cmte_zip             <chr> "64108", "20001", "73107", "66612", "46202", "20…
    ## $ cmte_dsgn            <chr> "U", "B", "U", "U", "U", "B", "B", "B", "U", "B"…
    ## $ cmte_tp              <chr> "Q", "Q", "N", "Q", "Q", "Q", "Q", "Q", "Y", "Q"…
    ## $ cmte_pty_affiliation <chr> "UNK", NA, NA, "UNK", NA, "UNK", "UNK", "UNK", "…
    ## $ cmte_filing_freq     <chr> "M", "M", "Q", "Q", "Q", "M", "M", "M", "M", "M"…
    ## $ org_tp               <chr> "C", "M", "L", "T", "M", "M", "L", "T", NA, "T",…
    ## $ connected_org_nm     <chr> NA, "AMERICAN MEDICAL ASSOCIATION", "TEAMSTERS L…
    ## $ cand_id              <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …

``` r
glimpse(contributions)
```

    ## Rows: 1,000
    ## Columns: 15
    ## $ cmte_id         <chr> "C00002766", "C00101485", "C90011156", "C00342113", "…
    ## $ amndt_ind       <chr> "N", "N", "N", "N", "N", "A", "N", "N", "N", "N", "N"…
    ## $ rpt_tp          <chr> "M8", "M7", "YE", "30G", "YE", "30G", "M7", "Q3", "Q3…
    ## $ transaction_pgi <chr> "G2016", "G2016", "G2016", "G2016", "G2016", NA, "G20…
    ## $ transaction_tp  <chr> "24K", "24K", "24E", "24K", "24E", "24C", "24K", "24A…
    ## $ entity_tp       <chr> "CCM", "CCM", "IND", "CCM", "IND", "ORG", "CCM", "IND…
    ## $ name            <chr> "THE NIKI TSONGAS COMMITTEE", "FRIENDS OF PAT TOOMEY"…
    ## $ city            <chr> "LOWELL", "OREFIELD", "SEATTLE", "TAYLORVILLE", "CINC…
    ## $ state           <chr> "MA", "PA", "WA", "IL", "OH", "CO", "NV", "PA", "OH",…
    ## $ zip_code        <chr> "01853", "18069", "981013001", "62568", "452291828", …
    ## $ transaction_dt  <date> 2016-07-05, 2016-06-06, 2016-09-21, 2016-10-31, 2016…
    ## $ transaction_amt <dbl> 5000, 2500, 28, 500, 8, 720, 2500, 27, 34, 2000, 25, …
    ## $ other_id        <chr> "C00433136", "C00461046", "S6NC00266", "C00521948", "…
    ## $ cand_id         <chr> "H8MA05143", "S4PA00121", "S6NC00266", "H2IL13120", "…
    ## $ tran_id         <chr> "14178158", "8100250", "VN7CZA3SJ92", "SB23.11481", "…

> Make sure that the row and column counts match\!
