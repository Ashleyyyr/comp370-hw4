# COMP 370 Homework 4

## Dataset Size

Commands used:

```bash
wc -c clean_dialog.csv
wc -l clean_dialog.csv
csvtool height clean_dialog.csv
```

The file is 4,870,970 bytes (approximately 4.87 MB). It contains 36,860 CSV rows including the header, so there are 36,859 dialogue records.

## Data Structure

Commands used:

```bash
csvtool width clean_dialog.csv
csvtool head 1 clean_dialog.csv
head -n 5 clean_dialog.csv
```

The dataset has four fields:

- `title`: the episode title, such as `Friendship is Magic, part 1`
- `writer`: the episode writer, such as `Lauren Faust`
- `pony`: the speaker or speaker label, such as `Twilight Sparkle` or `Narrator`
- `dialog`: the text spoken by the character

Each row after the header represents one dialogue record.

## Number of Episodes

Command used:

```bash
csvtool col 1 clean_dialog.csv | tail -n +2 | sort -u | wc -l
```

The dataset covers 197 unique episode titles. The command extracts the `title` column, removes the header, keeps only unique titles, and counts them.

## Unexpected Aspect

Command used:

```bash
csvtool col 3 clean_dialog.csv | grep " and " | sort | uniq -c | sort -nr | head -n 15
```

Some values in the `pony` field contain more than one speaker. Examples include `Fluttershy and Rainbow Dash`, `Pinkie Pie and Rarity`, and `Twilight Sparkle and Spike`. This could create problems for character-level analysis because an exact match for a single pony would not count dialogue records with a combined speaker label.

## Speaker Frequency

Commands used:

```bash
grep -c ',"Twilight Sparkle",' clean_dialog.csv
grep -c ',"Rarity",' clean_dialog.csv
grep -c ',"Pinkie Pie",' clean_dialog.csv
grep -c ',"Rainbow Dash",' clean_dialog.csv
grep -c ',"Fluttershy",' clean_dialog.csv
```

The exact speaker counts are:

- Twilight Sparkle: 4,745 lines
- Rarity: 2,660 lines
- Pinkie Pie: 2,833 lines
- Rainbow Dash: 3,072 lines
- Fluttershy: 2,109 lines

The percentages were calculated by dividing each pony's line count by the 36,859 dialogue records and multiplying by 100. The results are stored in `Line_percentages.csv`.

### Percentage Calculation

Command used:

```bash
awk 'BEGIN {
    total = 36859
    print "pony_name,total_line_count,percent_all_lines"
    printf "Twilight Sparkle,4745,%.6f\n", 100 * 4745 / total
    printf "Rarity,2660,%.6f\n", 100 * 2660 / total
    printf "Pinkie Pie,2833,%.6f\n", 100 * 2833 / total
    printf "Rainbow Dash,3072,%.6f\n", 100 * 3072 / total
    printf "Fluttershy,2109,%.6f\n", 100 * 2109 / total
}' > Line_percentages.csv
```