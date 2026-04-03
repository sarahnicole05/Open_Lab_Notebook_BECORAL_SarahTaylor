# Stupid R Code, J. Drake 20260401

DF = datafile/filename
Can load file from Import Dataset dropdown in RStudio. Make sure to set number columns to numerical. Setting one such columns sets all the other number columns 
Exported data will be in Finder->Jeana’s MacBook Pro->Macintosh HD->Users->Jeana Drake
If receive “incomplete final line on Filename(.fasta), go to final line of fasta file and hit Enter and re-save.

### Find out working directory
>getwd()

### Increase number of lines in output
>max.print=#
#can set at 100000 to be safe

### End “+” prompt 
Control + C

### Creating a data frame
my_dataframe <- data.frame(Name = c(“Name1”, “Name2”), Age = c(1,2))
#Creating a data table uses the same commands except must open library(data.table) first and use <- data.table command

### Extracting a subset of data from a table
>library(data.table)
>SubsetDF <- setDT(LargeDF) [ColumnName %chin% SmallDF$ColumnName)
>library(xlsx)
>write.xlsx(SubsetDF, file = “SubsetDF.xlsx”)

### Extracting a subset of sequences
>library(seqinr)
#make sure SequenceFile.fasta is in wd
#load DF with list of sequences IDs in subset. Make sure the sequence ID does NOT have >
>NewFastaName <- read.fasta(file = “SequenceFile.fasta”, seqtype = c(“AA”), as.string = FALSE, seqonly = FALSE, strip.desc = FALSE)
>NewFastaName2 <- NewFastaName[names(NewFastaName) %in% DF$ColumnName]
>write.fasta(sequences = NewFastaName2, names = names(NewFastaName2), file.out = “NewFastaName2.fasta”, open = “w”, nbchar = 60, as.string = FALSE)

### Find Poly-AA Stretches
>library(Biostrings)
>library(strings)
#Redirect console output to file in home directory (see commands below)
>NewFastaName <- readAAStringSet(“Filename.fasta”)
>CharacterVectorName <- as.character(NewFastaName)
>Fasta_Matches <- str_extract_all(CharacterVectorName, “[AApattern]{MinimumCount,}”)
#Pattern is any amino acids of interest (e.g., [DE])
#Count for pattern is range contained in curly brackets, {#,} gives lower bounds, {#1,#2} gives lower and upper bounds, {,#} gives upper bounds
>names(Fasta_Matches) <- names(NewFastaNames)
>print(Fasta_Matches)
#stop redirecting output to file

### Redirect All Console Output to File
>sink(file = “Filename.txt”)
#Run R commands, end with print command
>print(OutputOfLastCommand)
#Check output file
>sink()
#Stops redirecting output and returns to the console

### Merging files
>merge(DF1, DF2, by.x = c(“ColumnNameDF1”), by.y = c(“ColumnNameDF2”), all = TRUE)
#works even if there are different numbers of rows
>MergedDF <- merge(DF1, DF2, by.x = c(“ColumnNameDF1”), by.y = c(“ColumnNameDF2”), all = TRUE)
#Makes a new table that can be exported. The first command just prints the output in the terminal.

### AA Composition
>library(xlsx)
>library(“CHNOSZ”)
>SubsetDF <- read.fasta(“filename.fasta”, iseq = NULL, ret = "count", lines = NULL, ihead = NULL, start=NULL, stop=NULL, type="protein", id = NULL)
#OR
> SubsetDF <- read.fasta("filename.fasta", seqtype = c("AA"), as.string = FALSE, seqonly = FALSE, strip.desc = FALSE)
>write.xlsx(SubsetDF, file = “SubsetDF.xlsx”)
#If R gives “line # did not have # elements” error, use BioStrings to accurately read FASTA file
>library(Biostrings)
>DF_fasta_data <- readAAStringSet(“filename.fasta”)
#CHNOSZ does not seem to work with Biostrings-imported FASTA files
>DF_aa_counts <- letterFrequency(DF_fasta_data, letters = AA_ALPHABET)
>write.xlsx(DF_aa_counts, file = “DF_aa_counts.xlsx”)
#If “WARNING: An illegal reflective access operation has occurred” error is shown, ignore it and check to see if the file has been exported to the Home Directory anyway

### Isoelectric point (pI)
>library(seqinr)
>library(Biostrings)
>library(xlsx)
>library(Peptides)
>DF_fasta_proteins <- read.fasta(file = “filename.fasta”, seqtype = “AA”, as.string = TRUE)
#requires an empty row at the bottom of the fasta file
>DF_seq <- unlist(DF_fasta_proteins)
>DF_pi_values <- sapply(DF_seq, pI)
>DF_pI_export <- data.frame(Proteins = names(DF_pi_values), pI = DF_pi_values)
>write.xlsx(DF_pI_export, file = “DF_pI.xlsx”)

### Translate DNA to protein
>library(seqinr)
>translate(SEQ, frame = 0, sens = “F”, numcode = 1, NAstring = “X”, ambiguous = FALSE)
#frames are 0, 1, or 2; sens F is forward, R would be reverse; numcode 1 is standard genetic code; NAstring X translates aa’s for ambiguous bases as X; ambiguous FALSE doesn’t take ambiguous bases into account
#stop codons are printed as ‘*’
#list of numcodes (for mitochondrial, etc) are at the translate can page
#the above translates one sequence, see below to translate from fasta file in home drive
>FASTA <- read.fasta(file = “SequenceFile.fasta”, seqtype = c(“DNA”), as.string = FALSE, seqonly = FALSE, strip.desc = FALSE)
>TRANSLATED_FASTA <- translate(seq = FASTA, etc)
#Might require a carriage return after last line

### Export table larger than xlsx limit (1,048,576 rows)
>write.table(DF, file = “DF”, sep = “\t”, rownames = FALSE)

### Export table when RStudio can’t install xlsx package
write.table(DF, file = “DF.txt”)

### Boxplot
>boxplot(NumericalColumn~FactorColumn, data = DF, cex.axis = #, cex.lab = #, ylim = c(#,#), ylab = “Text”, horizontal = TRUE, col = #)
#cex.axis, cex.lab, cex.main is an expansion factor; >1 maxes larger, <1 makes smaller
#ylim (or xlim) changes axis limit; can be reversed by switching values (xlim = c(##, #)) where values are related to axis size, not necessary to values in the spreadsheet (I.e., depths of 10 and 30 m might have c(##, #) values of c(2.5, 0.5))
#ylab (or xlab) changes axis label
#horizontal rotates axes; must then rename axis labels
#col sets the color according to the codes below in Mean Centered Cluster Plots
#For multiple colors, col = c(“#”, “#”, “#”, etc) above
#or
>boxplot(DF$NumericalColumn, DF$FactorColumn)
>boxplot(DF$Numerical ~ DF$Factor)

### Boxplot for outliers
>boxplot(DF$NumericalColumn, outcol = “red”, cex=#)
#outcol sets the color of outlier circles
#cex sets the outlier circle size

### Different order to box plot
>o = ordered(DF$FactorColumn, levels = c(“Factor1”, “Factor2”, etc))
>boxplot(NumeralColumn~o, data = DF)

### Variance of all
>var(DF$NumeralColumn)

### Variance of a subset
>var(DF$NumeralColumn[DF$FactorColumn==“factorterm”])

### Fligner test
>fligner.test(DF$NumeralColumn~DF$FactorColumn)
#test for homogeneity of variance

### Shapiro test
>shapiro.test(DF$NumeralColumn[DF$FactorColumn==“factorterm”])
#test for normal distribution
#can add [1:5000] immediately after FactorColumn and continue in groups of 5000 if >5000 rows

### One way ANOVA
>oneway.test(DF$NumeralColumn~DF$FactorColumn)
#independent samples, normal distribution, same variance

### Tukey HSD post-hoc for ANOVA
>data.lm <- lm(DF$DependentVariable~DF$IndependentVariable, data = DF)
>data.av <- aov(data.lm)
>summary(data.av)
>data.test <- TukeyHSD(data.av)
>data.test

### Kruskal-Wallis test
>kruskal.test(DF$NumeralColumn~DF$FactorColumn)
#for non-normal distribution or inhomogeneous variance

### Dunn’s test post-hoc for Kruskal-Wallis
>library(dunn.test)
>dunn.test(DF$DependentVariable, DF$IndependentVariable, method=“bonferroni”)

Alternatively:
>library(FSA)
>PT = dunnTest(DependentVariable ~ IndependentVariable, data=DF, method=“bonferroni”)

### T-test
>t.test(NumericalColumn~FactorColumn, data = DF, mv = 0, alt = “two.sided”, conf = 0.95, var.eq = F, paired = F)
#mv = 0 is null hypothesis that mean=0.

### Wilcoxin test
>wilcox.test(DF$NumeralColumn~DF$FactorColumn)
#non-parametric t-test

### Pairwise Wilcoxin
>pairwise.wilcox.test(DF$NumeralColumn, DF$FactorColumn, paired = FALSE, p.adjust.method=“BH”)
#can use default p.adjust method

### Rosner’s Test for Outliers
>library(readr)
>library(outliers)
>library(EnvStats)
#See box plot for outliers above
>rosnerTest(DF$NumericalColumn, k=#, warn=TRUE
#k=# is number of red outlier circles plus one. Can ballpark if you can’t tell bc there are so many
#If k=# value is too low, the same number of outliers as the # will be returned
#$all.stats information will list the outliers first and will give the row number on which they fall

### P-values
#all pheatmap library except for pheatmap
>DF_clean = DF %>%
>select(“ColumnName”, starts_with(“Column1”, “Column2”)%>%
>as.data.frame()
#ignore select if DF only has the information to be graphed
#as.data.frame converts from tibble to dataframe
>DF_clean2 <- DF_clean[, -1]
#removes row name column
>rownames(DF_clean2) <- DF_clean[, 1]
#moves row name column over to dataframe’s rownames
>pValues <- apply(DF_clean2, 1, function(x) t.test(x[Column#:Column#],x[Column#:Column#])$p.value)
#first range of column numbers is 1st group, second range is second group
>library(xlsx)
>write.xlsx(as.data.frame(pValues), file = “filename.xlsx”)

### Fisher’s Exact Test
>MatrixName = rbind(c(x,x,x), c(x,x,x), c(x,x,x), etc); MatrixName
>fisher.test(MatrixName, workspace = 2000000))

### Histogram
>hist(df$NumeralColumn)

### Pie Chart (base)
>Category1 <- c(“Name1”, “Name2”)
>Category2 <- c(1,2)
>DataName <- data.frame(Category1, Category2)
#Can see data frame with the following command, but not necessary
>DataName
>mycolors <- c(“red”, “yellow”)
>pie(DataName$Category2, labels = Category1, cex = #, main = “Title”, col = mycolors)
#cex changes the label font size
#base pie command keeps order of Category1
>pie(DataName$Category2, labels = NULL, cex = #, main = “Title”, col = mycolors)
#Removes label names, replaces them with numbers that can be removed in Illustrator
>legend(“right”, legend = Category1, fill = mycolors, cex = #)
#Add legend. Can use bottomright, bottom, bottomleft, left, or combos with top.
#Legend overwrites part of pie chart. Generate pie chart w/ and w/o legend and crop if needed

### Pie Chart (ggplot2)
>library(ggplot2)
#make data frame as above for base pie chart
>p <- ggplot(DataName, aes(x = “”, y = Category2/value, fill = Category1/category)) + geom_bar(width = 1, stat = “identity”)
#ggplot doesn’t have a pie chart so start as a bar chart and the x=“” creates a fake x-axis
#fill fills in the slices to assign colors later
>pie_chart <- p + coord_polar(theta = “y”)
#converts the bar chart (Cartesian coordinates) to polar coordinates as a circle
>pie_chart_enhanced <- pie_chart +
      theme_void() +
      labs(title = "Distribution of Categories") +
      geom_text(aes(label = category), 
                position = position_stack(vjust = 0.5))
#Not required but enhances the appearance
>pie_chart_enhanced
#Displays pie chart
#ggplot2 pie command reorder Category1 alphabetically

### Scatterplot
>plot(df$x-axis, df$y-axis, col = “color”, pch = ##, xlim = range(c(#,#)), xlab = “Label”)
#pch gives scatter as points and value changes the size, with smaller values yielding larger size, max value of 25
>points(df$x-axis, df$y-axis, col = color2”, pch = ##)
#Adds second set of data on same axes
>legend(“toplight”, legend = c(“Label1”, “Label2”), col = c(“color1”, “color2”), pch = 3#

### Heatmap
>library(tidyverse)
>library(magrittr)
>library(pheatmap)
> library(RColorBrewer)
> library(rio)
> library(dplyr)
>DF_clean = DF %>%
>mutate(newcolumnname = paste0(column1, “:”, column2) %>%
>select(“newcolumnname”, starts_with(“Numbercolumn1”, etc) %>%
>as.data.frame()
#ignore mutate command if gene ID and annotation are already in the same column or if that information isn’t needed
#ignore select if DF only has the information to be graphed
#as.data.frame converts from tibble to dataframe
>DF_clean2 <- DF_clean[, -1]
#removes row name column
>rownames(DF_clean2) <- DF_clean[, 1]
#moves row name column over to dataframe’s rownames
>boxplot(DF_clean2, las = 2)
#checks distribution of data
#las = 2 makes sample label perpendicular
>DF_clean2_log10 = log10(DF_clean2)
>boxplot(DF_clean2_log10, las = 2)
#minimizes variance
>pheatmap(DF_clean2 or DF_clean2_log10), border_color = NA, fontsize_row = #, fontsize_col = #, cluster_rows = F, show_rownames = F)
#cluster_rows = F orders rows according to how they are in the dataframe, like if making a heat map to match a ranking graph
#show_rownames = F removes rownames if making a giant heatmap of tons of genes
#to set the order of the columns but keep their dendrogram, do the following:
>library(dendextend)
>phtmap <- pheatmap::pheatmap(DF_clean2)
>col_dend <- phtmap[[2]]
>col_dend <- dendextend::rotate(cold_dend, order = c(“Column1”, “Column2”, etc))
>pheatmap(DF_clean2, cluster_cols=as.hclust(col_dend), all the other pheatmap stuff)
#assigned column order must be possible given the dendrogram or it will give an error or just not change the order.

### PCA
>library(tidyverse)
>library(magrittr)
> library(RColorBrewer)
> library(rio)
> library(dplyr)
#prcomp is standard in R
#Load file
#If file is small, can load with samples in rows and genes/proteins in columns. Remove sample name column as all data must be numerical and R won’t move row names in 1st column to row names using pheatmap commands
#If file is large (like normalized and transformed deseq output) and can’t be transposed first in excel bc it exceeds the column limit, load as a normal file with samples as columns and genes as rows. Remove first row with sample names before loading
>DF = as.martrix(DF)
#if file has samples as columns and genes as rows, transpose first using the following command. If file loaded already transposed, skip it
>DF_trans <- t(DF)
>DF_PCA <- prcomp(DF, center = TRUE, scale = TRUE
#center shifts the variables to be zero centered
#scale shifts the unit variance before the analysis takes place. Scaling is advisable by the rdocumentation.
#if transpose command was used, make sure to do prcomp on DF_trans rather than DF
>summary(DF_PCA)
#Gives SD, % of variance, and cumulative % of variance explained by each PC
>str(DF_PCA)
#gives information about the PCA object itself
>rownames(DF) <- c(“Sample1”, “Sample2”, etc)
#makes position of each sample in PCA plot show up as sample name
>library(devtools)
>library(ggbiplot)
>ggbiplot(DF_PCA, var.axes = FALSE, labels=rownames(DF), labels.size=#)
#4 is a good size for row name font
#var.axes removes the arrows for each factor placing the samples in the PCA. Each factor is a gene or protein
#if xlim or ylim need to be adjusted or to add label or change background color, make the ggbiplot its own object
>DF_PCA_obj <- ggbiplot(DF_PCA, var.axes = FALSE, labels=rownames(DF), labels.size=#)
>DF_PCA_obj + coord_cartesian(xlim = c(#,#), ylim = (c(#,#) + theme_bw(), ggtitle(“Title”) + theme(plot.title = element_text(hjust = 0.5))
#theme_bw leaves gray lines from tick points but makes background white and puts solid black lines around the plot
#theme(plot.title…) centers the title. The default is to have it left-justified for some stupid reason

### nMDS
>library(vegan)
>m_DF = as.matrix(DF)
#just values, no row names or other columns
>DF_nmds = metaMDS(m_DF, distance = “dissimilarityindex”, maxit = 200, sfgrmin = 1e-7)
#bray-curtis is default
#maxit increases the # of iteration
#sfgrmin decreases scale factor of the gradient, add this if getting the error ‘scale factor of the gradient <sfgrmin’
#if nmds doesn’t converge, try the following:
>DF_nmds2 = metaDMS(m_DF, distance = “dissimilarityindex”, previous.best = DF_nmds, maxit = 200)
#if nmds still doesn’t converge, give up and do PCA

### Make mean-centered cluster plots
>maxplot(DF, type = “1”, pch = 1, col = #, ylab = “labelname”, xlay = “labelname”, xaxis = c(#,#), main = “titlename”)
#type is line type assignment, 1=solid line
#col is color assignment, 1=black, 2=pink, 3=green, 4=blue, 5=cyan, 6=magenta, 7=yellow, 8=gray, 9=black, 10=dark pink
#can also use the HEX color code in “” “”
#to make lines different colors, have each group as a separate tab
#after maxplot, do the following:
>maxlines(DF, type = “1”, pch = 1, col = different#thaninmaxplot, lay = #forlinetype, lwd = #forlinewidth)

### Transpose data.frame or data.matrix
Transposedf <- t(df)

### Make a list for MOFA
Listname=list(title1=as.matrix(df1),title2=as.matrix(df2))

### Make comma separated values onto separate lines
>library(tidyverse)
>Sep_DF <- DF %>% tidyr::separate_rows(“Column name”, sep = “,”)
>library(xlsx)
>write.csv(Sep_DF, file = “Filename.csv”)
#Replace , with whatever separator is used.
#Open in excel and save as tab separated text file
#For Blast2GO, change suffix to .annot 

### Signal peptide prediction
>install.packages(“signalHsmm”)
>library(signalHsmm)
#Read in fasta file per above, making sure that stop codon ‘*’ is removed from all sequences
>FASTA_signalP <- run_signalHsmm(FASTA[1:n])
#n = highest number of sequences desired
>library(xlsx)
>FASTA_signalP_table <- pred2df(FASTA_signalP)
>write.xlsx(FASTA_signalP_table, file = “FASTA_signal_table.xlsx”)

### Inserting carriage return in text
#This is more for text/BBEdit
#For eDNA sequencing core output for SNPs
#Open in Excel
>=(cell with “>”)&(cell with sequence)
#Drop to all sequences
#Control-c
#Open in BBEdit
#Right-click
#Paste-values
#Control-f
>Replace >
>Replace with >control-return
#Replace all

### Dendrogram
>library(ape)
>DendroName <- read.tree(text = “NEWICK FILE TEXT PASTED IN”)
#Ensure that the tree string ends in a semi-colon.
#Make sure that brackets are matched. Some text editors, such as RStudio or Notepad++, contain built-in syntax highlighting that will highlight the counterpart to each parenthesis as you type.
#Look out for ((double brackets)) or brackets enclosing a single node: the resultant ‘invisible nodes’ can cause R to crash. >plot(DendroName)
#write.dendrogram from ape package doesn’t seem to work (could be RStudio version on my very old laptop)
>plot(DendroName, show.node.label = TRUE)
#shows bootstrap values at nodes
>nodelabels()
#labels the nodes with their number
>tiplabels()
#labels the tips with their number
#If the values beyond the decimal point should be shortened or changed, must be done manually in the Newick file; the node values to be changed are before the : in the format #.########(node value):#.########(node length)
>rt.DendroName# <- rotate(DendroName,node#)
>plot(rt.DendroName#
#rotates at a node of Node#
#Node #s seem to start at the top of the dendrogram
>rr.DendroName# <- root(DendroName,node=#)
>plot(rr.Dendroname#)
#reroot at a node of Node#

### Superscript & Subscripts
>boxplot(NC~FC, data =DF, ylab = expression(paste(“Text“^”Text”*” Text”)))
#^ raises the next alphanumeric, * lowers the next alphanumeric back to normal
#[] around alphanumeric makes the contents subscripted, * lowers the next alphanumeric back to normal

### Greek Letters and other symbols (Mac)
#RStudio allows copy/paste of symbols and emojis

### Taxonomizr
>library(taxonomizr)
>prepareDatabase(‘accessionTaxa.sql’)
SQLite database accessionTaxa.sql already exists. Delete to regenerate
#Do NOT delete the database and associated files
[1] “accessionTaxa.sql”
>taxaYOURSAMPLE<-getId(c(“sps 1”, “sps 1”, “sps 3”, “etc”), ‘accessionTaxa.sql’)
#Give whatever name you want for taxaYOURSAMPLE
#Each Latin name must be in double quotations and separated by a comma
#There can be a space between the names and the commas
#Just copying the vertical list of Latin names from the Organism column of the Blast output doesn’t work
#RStudio can only handle 4091 characters per line. To get around this for long lists of Latin names, highlight names in the alphabetized list (see Megablast .csv instructions above) up to 4000 characters inclusive of quotes and commas (BBEdit shows character count in the lower right side of the file border) starting with a double quote before a species name and the comma after a species name. Then hit enter. This yields a ‘+’ and starts a new line of the same command. You can then copy in the next batch of species names up to ~4000 characters and continue with species name batches until you run through your entire non-redundant list. On the last line of the command, include the “), ‘accessionTaxa.sql’)” to close out the command.
>DF<-getTaxonomy(taxaYOURSAMPLE, ‘accessionTaxa.sql’)
>write.table(DF, file = “DF.txt”)
#This command writes a txt file to the home directory containing the taxonomy assignments for all species across all lines of the command above that are separated by ‘+’

### Multiple Sequence Alignment
>if (!require("BiocManager", quietly = TRUE))
+     install.packages("BiocManager")
>BiocManager::install("msa")
>library(msa)
#Make sure .fasta file is in Users directory
>msa(“Filename.fasta”, method = c(“ClustalOmega”), type = “protein”
#Output to cluster format is not yet resolved.

### Mapping Peptides to Proteins
>library(protti)
>library(devtools)
>library(tidyverse)
>library(dplyr)
>library(magrittr)
#Open .xlsx file of Accession# + Peptide
#Open .xlsx file of Accession# + Protein
>Merged_ProtPep <- merge(DF1, DF2, by.x = c(“DF1AccessColumn”), by.y = c(“DF2AccessColumn”), all = TRUE)
>write.table(Merged_ProtPep, file = “Merged_ProtPep.txt”)
#Open Merged_ProtPep
>ProtPepDF <- as.data.frame(Merged_ProtPep)
>PepMap <- find_peptide(data = ProtPepDF,protein_sequence = ProteinColumn,peptide_sequence = PeptideColumn)
>write.table(PepMap, file = “MappedPeptides.txt”)
