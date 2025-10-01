# How I have been writing my cover letters

2025-09-30

Something I've cooked up in my job search is quickly writing cover letters for companies. I wanted a way to write a *personalized* letter, but also not spend that much time rewriting and formatting ever time.

What I've come up with is have a simple outline in latex and use a bash script to compile the output. Here is the script:

```
# parsing the input
if [[ "$#" -eq 0 ]]; then
  echo "usage: compile.sh --company=\"Google\" --role=\"Software Engineer\""
  echo "Please provide --role and --company"
  exit 1
else
  # grabs the role and company flag values and assigns them to a variable
  # I can't find where I got it from, real lesson of always credit something when you find it
  while [[ "$#" -gt 0 ]]; do
      case $1 in
          --role*) role="${1#*=}";;
          --company*) company="${1#*=}";;
          *) echo "Unknown parameter passed: $1";;
      esac
      shift
  done
  touch temp.tex
  # My template file is stored in 'Hughes_Cover_Letter2025.tex'
  echo "\\def\\company{$company}\\def\\role{$role}\\input{Hughes_Cover_Letter2025}" > temp.tex
  latexmk -pdf -quiet temp.tex 
  # Name for output file incase company name has spaces
  company_underscore=$(echo $company | sed 's/ /_/g')
  cp temp.pdf "./pdf/Hughes_Cover_Letter2025_$company_underscore.pdf"
  latexmk -C
  rm temp.*
  # Preview the file to double check if there are any errors
  zathura "./pdf/Hughes_Cover_Letter2025_$company_underscore.pdf"
fi
```

So all I have to do is run
`./compile.sh --company="Company Name" --role="Software Engineer"`
and my cover letter is written to Hughes_Cover_Letter2025_Company_Name.pdf
This could be extended to have more variables if I wanted to make it more *personalized* to each company, but I don't want to right now. 
Most of the time it takes about one minute to write some extra stuff in the latex file, and then I make it in an extra 15 seconds. Its not great but is enough for me.