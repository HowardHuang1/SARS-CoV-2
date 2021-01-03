# SARS-CoV-2

The aim of this project is compare mutations across strains of SARS-CoV-2 and to predict the country of origin for each viral mutation. This will allow researchers to better understand how the SARS-CoV-2 genome changes over time and may help unlock a cure for the coronavirus.

### Background
SARS-CoV-2 stands for severe acute respiratory syndrome coronavirus 2 and belongs to the bigger coronavirus family.
In 2002, there was an outbreak of SARS-CoV and in 2012, there was an outbreak of MERS-CoV. SARS-CoV-2 first emerged in 2019 in Wuhan China. The virus uses a recepter known as ACE2 to bind to human cells and primarily targets the respiratory tract.

All three of these viruses have their origins in bats. The sequences from U.S. patients are similar to the one that China initially posted, suggesting a likely single, recent emergence of this virus from an animal reservoir. 

### Visualizers
[Multiple Sequence Alignment Viewer](https://www.ncbi.nlm.nih.gov/projects/msaviewer/?appname=ncbi_msav&openuploaddialog) and [UCSC Genome Browser](https://genome.ucsc.edu/cgi-bin/hgTracks?db=wuhCor1&lastVirtModeType=default&lastVirtModeExtraState=&virtModeType=default&virtMode=0&nonVirtPosition=&position=NC_045512v2%3A1%2D29903&hgsid=989829605_h4eZoduPwgHnTDuUdX9PCReUleqx) are great tools to help visualize RNA nucleotide sequences.

### Training

The raw data is given as genome sequences in the form of `.fasta` files. We can read these files using a library called SeqIO from a collection of biology-related tools called BioPython.

We can compute the similarity score between SARS-CoV-2 and the RTG13 bat coronavirus with
```
sequence_bat = np.array(sequences[0]) 
sequence_cov2 = np.array(sequences[1])

length_cov2 = sum(sequence_cov2!='-')
n_bases_different = sum(sequence_bat!=sequence_cov2)
n_bases_same = sum(sequence_bat==sequence_cov2)

print("1. Length of SARS-CoV-2 genome: ", length_cov2)
print("2. # of bases that differ from RTG13: ", n_bases_different)
print("3. Percent similarity: %", 100*n_bases_same/length_cov2)
```
To categorize the countries of origins for the different viral strains we can first classify each country into three groups: North America, Oceania, and Asia.
The logistic regression model can be defined by the following function
```
lm = linear_model.LogisticRegression(
    multi_class="multinomial", max_iter=1000,
    fit_intercept=False, tol=0.001, solver='saga', random_state=42)

X_train, X_test, Y_train, Y_test = model_selection.train_test_split(X, Y, train_size=0.8)

lm.fit(X_train, Y_train)
```
Finally, we can run `Y_pred = lm.predict(X_test)` to run our prediction using the model and compute the accuracy with `accuracy = 100*np.mean(Y_pred == Y_test)`
