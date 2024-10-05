## FlowLogParser
Flow Log Parser - Illumio Technical assessment

This program processes AWS VPC flow logs and maps each flow to a tag based on a lookup table of `dstport` and `protocol`. It outputs the count of logs per tag and per port/protocol combination. 

## Requirements

- The program is written in Python completely.
- No external dependencies or libraries are required or need to be installed


## Steps to run the program

1. Clone the repository to your local machine:
   
   git clone https://github.com/Sanket2596/FlowLogParser.git
   cd FlowLogParser

2. Run the command: python flow_log_parser.py.

-> The program will generate two output files:
1. tag_counts.csv: Count of matches for each tag.
2. port_protocol_counts.csv: Count of matches for each port/protocol combination.

# Assumptions that were made while running the program:

1) The program only supports the default flow log format, version 2, based on the given example amd it was assumed that this is the only valid version logs that will be supported. Hence, the progam does not handle custom formats or other versions.

2) The protocol field is case-insensitive, meaning "TCP" and "tcp" are treated as equivalent.
Explanations for assumption -> The program converts all protocol values to lowercase before matching, so entries in the flow log can have "TCP", "tcp", or any other case variation, and they will still be matched correctly.

3) Any log entry that does not match any dstport/protocol combination in the lookup table is counted as "Untagged" and grouped accordingly.
Explanation for assumption -> w log entry doesn’t have a corresponding tag in the lookup table, it’s classified as "Untagged" in the final output. This makes sure that no entry in the logs is left unaccounted for and can be categorized into some of the category.

4) The 7th row in the log entry file is the protocol number which defines either it is tcp or udp checking the number.
-> Based on the sample output provided it was assumed that 6 -> tcp and 17 -> udp protocol and thus the conditions were handled accordingly and if any other number that 6 or 17 then protocol was categorized into unkown category.

5) The program assumes the fields in each flow log entry follow the exact ordering as in the example logs (e.g., dstport at index 5 and protocol at index 7).
-> Based on the provided sample output the program written mainly relies on the fixed positioning of these fields for parsing. If the log format changes or fields appear in a different order, the code will fail or produce incorrect results. SO the ordering of the log fields is assumed to be in the given order and will always remain in that order.