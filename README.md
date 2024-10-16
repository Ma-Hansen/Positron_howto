# Positron_howto
Some notes on how to get started with Positron if your goal is to mix code chunks of different programming languages (in my case, mostly R and Julia, and some Python). The notes will be extended by information about converting objects from one programming language to another, solutions for common errors, Python-specific chunks, etc. I will also add an example script. Feel free to extend these notes if you are also a fresh user of Positron and have tried combining several programming languages. 

Here, we will assume that you have Positron, Julia, and R installed, as well as the Positron extensions for Julia language support and Quarto.

### Basics:

- You can use R in Positron the same way (mostly) that you use it in RStudio:  
`New > New File… > R File`
- You can mix Julia & R code chunks (or Julia, R, and Python…) in a Quarto document (`.Qmd`). If you want to do that, keep in mind that your document will be Julia-based in general. That is, the engine that you use in Quarto will be Julia, similar to the engine you use in your RMarkdown documents being R. Hence, the basic logic you need to follow is Julia’s.
- You need to install **RCall** (`add RCall`) and load the package (`using RCall`) before you can run R code in your Qmd.
- It seems that you cannot simply load any package from your R library in Positron if you want to execute code chunk-wise. If your code is executed rendering a script with Quarto, there does not seem to be a problem accessing your previously installed R packages from the library path that you would also access via, e.g., RStudio. However, if you want to run chunks of code in a Quarto document in Positron, your packages will be managed project-wise:
 - What you **cannot** do: `library(tidyverse)`
 - What you **should** do instead: Install all packages that you need for the project you are currently working in (e.g., via `install.packages()`, using `pak`, or whatever package manager you usually use), then call them from the library.
 - You **cannot** manage your R packages in the Qmd file! Quarto can access your “standard R library” when you render the document (and only then), but you cannot manage your R packages from Quarto. You need to open an R file in Positron to install, remove, and update your R packages! For Julia, this seems to be different — you can successfully manage your Julia packages via Julia code chunks in the Qmd.

Once you have set up your Julia and R packages, you are ready to go! 

Insert Julia code chunks with:
````
```{julia}  
# some Julia code  
```
```` 

Insert R code chunks with:
````
```{julia}  
R"""  
# some R code  
"""  
```
```` 

Insert just one line of R code with:
````
```{julia}  
# some Julia code  
R"some R code"
``` 
````

### How to convert an existing Rmd file to Qmd:

- **Optional**: Create a new project: `New > New Project > R Project`
- Open the Rmd file that you want to convert: `Open > Open File…`
- Change your yaml specifications in the beginning of the Rmd document. Most importantly, make sure that you take care of the following parts:
 - Add `engine: julia`
 - If you want to specify the format already, change `output` to `format: html` (or pdf, or whatever document type you wish to get after rendering later on)
- In your R setup chunk, add the following line of code:
````
```{r}  
knitr::convert_chunk_header("name_of_current_Rmd_file.Rmd", "name_for_converted_Qmd_file.Qmd", type="yaml")  
```
````
- Render the document with Quarto: Press `CTRL + Shift + P` and search for the command **Quarto: Render Document**, when you execute it. Quarto should start rendering and the progress should be shown in the console (the prerequisite is, of course, that your code runs in principle).

Once rendering is complete, you should see the Qmd document in the working directory where the Rmd file is stored, and you should see a preview of your output file in the Viewer panel in Positron (right side, if you haven’t configured Positron otherwise).
