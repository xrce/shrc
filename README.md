> [!TIP]
> Copy code to .bashrc to use any customization

#

- ### Auto ls after cd
    To enhance your navigation efficiency in the terminal, you can set up your environment so that the `ls` command runs automatically whenever you change directories with `cd`. This provides an immediate view of the contents of the new directory, saving you the extra step of typing `ls` each time.

    **Code:**
    ```sh
    cdls() {
        cd "$@" && ls
    }

    alias cd='cdls'
    ```
    **Usage:**
    ```sh
    cd
    ```

- ### Easiest way to run any program
    By customizing the `command not found` handler in your shell, it automatically detects the file type and executes the corresponding interpreter. This means you can simply type the filename of your script without worrying about which interpreter to use.

    **Code:**
    ```sh
    command_not_found_handle() {
        local cmd="$1"
        shift
        local args="$@"
        declare -A interpreters

        interpreters=(
            ["py"]="python"
            ["php"]="php"
            ["js"]="node"
            ["sh"]="bash"
            ["rb"]="ruby"
            ["pl"]="perl"
            ["java"]="java"
            ["cpp"]="g++"
            ["c"]="gcc"
            ["go"]="go run"
            ["rs"]="rustc"
            ["swift"]="swift"
            ["lua"]="lua"
            ["r"]="Rscript"
            ["scala"]="scala"
            ["hs"]="runhaskell"
            ["ts"]="ts-node"
            ["kt"]="kotlinc -script"
            ["cs"]="csharp"
            ["fs"]="fsharp"
            ["clj"]="clojure"
            ["groovy"]="groovy"
            ["ml"]="ocaml"
            ["pro"]="gprolog"
            ["erl"]="erl -noshell -s"
            ["exs"]="elixir"
            ["jl"]="julia"
            ["v"]="v run"
            ["nim"]="nim compile --run"
            ["dart"]="dart"
            ["cr"]="crystal run"
            ["adb"]="gnatmake"
            ["d"]="dmd"
            ["f90"]="gfortran"
            ["vbs"]="cscript //Nologo"
            ["awk"]="awk -f"
            ["m"]="octave --silent"
            ["ps1"]="pwsh"
            ["tcl"]="tclsh"
            ["rkt"]="racket"
            ["lisp"]="clisp"
            ["scm"]="guile"
            ["hx"]="haxe --run"
            ["purs"]="purs compile"
        )

        local file_found=false
        local interpreter=""

        for ext in "${!interpreters[@]}"; do
            if [[ -f "${cmd}.${ext}" ]]; then
                file_found=true
                interpreter=${interpreters[$ext]}
                if [[ -n "${interpreter}" ]]; then
                    if [[ "${ext}" == "java" ]]; then
                        javac "${cmd}.${ext}" && java "${cmd}" $args
                    elif [[ "${ext}" == "cpp" ]]; then
                        g++ "${cmd}.${ext}" -o "${cmd}" && ./"${cmd}" $args
                    elif [[ "${ext}" == "c" ]]; then
                        gcc "${cmd}.${ext}" -o "${cmd}" && ./"${cmd}" $args
                    elif [[ "${ext}" == "rs" ]]; then
                        rustc "${cmd}.${ext}" && ./"${cmd}" $args
                    elif [[ "${ext}" == "kt" ]]; then
                        kotlinc -script "${cmd}.${ext}" $args
                    elif [[ "${ext}" == "adb" ]]; then
                        gnatmake "${cmd}.${ext}" -o "${cmd}" && ./"${cmd}" $args
                    elif [[ "${ext}" == "d" ]]; then
                        dmd "${cmd}.${ext}" -of="${cmd}" && ./"${cmd}" $args
                    elif [[ "${ext}" == "f90" ]]; then
                        gfortran "${cmd}.${ext}" -o "${cmd}" && ./"${cmd}" $args
                    else
                        ${interpreter} "${cmd}.${ext}" $args
                    fi
                    return 0
                fi
            fi
        done

        if [[ -d "${cmd}" ]]; then
            cd "${cmd}"
            return 0
        fi

        if [[ "${file_found}" = false ]]; then
            echo "There's no ${filename}"
        fi

        return 127
    }
    ```
    **Usage:**
    ```sh
    {filename}
    ```
    **Example:**
    ```sh
    mycode
    ```
