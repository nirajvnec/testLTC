git log --author="niraj.kumar.4@credit-suisse.com" --shortstat | grep "files changed" | awk '{print $1}' | awk '{total+=$1} END {print total}'
