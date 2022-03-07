#!/bin/bash

nc="\033[0m" # no color
text_color="\033[0;32m" # green
cat_color="\033[1;32m" # light green
bar_color="\033[0;34m" # blue

#        |\      _,,,---,,_     
#  ZZZzz /,`.-'`'    -.  ;-;;,_ 
#       |,4-  ) )-,_. ,\ (  `'-'
#      '---''(_/--'  `-'\_)     

function get_title {
    user=${USER:-$(id -un)}
    
    hostname=${HOSTNAME:-${hostname:-$(hostname)}}
    if [ -z "$hostname" ]; then
        read -r hostname < /etc/hostname
    fi

    echo ${text_color}${user}${nc}@${text_color}${hostname}
}

function get_clean_title {
    user=${USER:-$(id -un)}
    
    hostname=${HOSTNAME:-${hostname:-$(hostname)}}
    if [ -z "$hostname" ]; then
        read -r hostname < /etc/hostname
    fi

    echo ${user}@${hostname}
}

function get_bar {
    bar=""
    title=$(get_clean_title)

    for ((i=1;i<=${#title};i++)); do
        bar="${bar}━"
    done

    echo ${bar}
}

function get_os {
    if [ -f /etc/os-release ]; then
        while IFS='=' read -r key val; do
            case $key in
                (PRETTY_NAME)
                    distro=$val
                ;;
            esac
        done < /etc/os-release
    fi

    distro=${distro##[\"\']}
    distro=${distro%%[\"\']}

    echo ${distro}
}

function get_uptime {
    IFS=. read -r s _ < /proc/uptime

    d=$((s / 60 / 60 / 24))
    h=$((s / 60/ 60 % 24))
    m=$((s / 60 % 60))

    output=""

    if [ ! $d -eq 0 ]; then
        output="${output}${d}d "
    fi
    if [ ! $h -eq 0 ]; then
        output="${output}${h}h "
    fi
    output="${output}${m}m"

    echo ${output}
}

function get_wm {
    if [ -z "$DISPLAY" ]; then
        return
    fi

    id=$(xprop -root -notype _NET_SUPPORTING_WM_CHECK)
    id=${id##* }
    wm=$(xprop -id "$id" -notype -len 25 -f _NET_WM_NAME 8t)

    wm=${wm##*_NET_WM_NAME = \"}
    wm=${wm%%\"*}

    echo ${wm}
}

function get_shell {
    echo ${SHELL##*/}
}

ascii_offset="                              "
ascii_line_1="       |\      _,,,---,,_     "
ascii_line_2=" ZZZzz /,\`.-'\`'    -.  ;-;;,_ "
ascii_line_3="      |,4-  ) )-,_. ,\ (  \`'-'"
ascii_line_4="     '---''(_/--'  \`-'\_)     "

printf "${ascii_offset}  $(get_title)\n"
printf "${ascii_offset}  ${bar_color}$(get_bar)${nc}\n"
printf "${cat_color}${ascii_line_1}  ${text_color}▪ os  ${nc}$(get_os)\n"
printf "${cat_color}${ascii_line_2}  ${text_color}▪ wm  ${nc}$(get_wm)\n"
printf "${cat_color}${ascii_line_3}  ${text_color}▪ sh  ${nc}$(get_shell)\n"
printf "${cat_color}${ascii_line_4}  ${text_color}▪ ut  ${nc}$(get_uptime)\n"
echo
