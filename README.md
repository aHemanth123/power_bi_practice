# power_bi_practic

# Dax 

Sales Volume Numeric = 
VAR TextValue = 'phone search'[sales_volume]
VAR CleanedText = SUBSTITUTE(SUBSTITUTE(TextValue, " bought in past month", ""), "+", "") 
VAR NumericValue = 
    IF(
        ISBLANK(CleanedText), 
        0, -- Convert NULL to 0
        IF(
            RIGHT(CleanedText, 1) = "K", 
            VALUE(LEFT(CleanedText, LEN(CleanedText) - 1)) * 1000, -- Convert "K" to *1000
            VALUE(CleanedText) -- Directly convert if no "K"
        )
    )
RETURN NumericValue




 #"Sales Volume Cleaned" = Table.AddColumn(#"Added Custom", "sales_volume_numeric", each 
        let 
            textValue = [sales_volume],
            cleanedText = Text.Replace(Text.Replace(textValue, " bought in past month", ""), "+", ""),
            isK = Text.EndsWith(cleanedText, "K"),
            numericValue = try if isK then Number.From(Text.Start(cleanedText, Text.Length(cleanedText) - 1)) * 1000 
                            else Number.From(cleanedText)
                        otherwise 0
