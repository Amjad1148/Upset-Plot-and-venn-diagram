# Upset-Plot-and-venn-diagram

```
                  library(tidyverse)
                  library(UpSetR)
                  
                  # Read Orthogroups.tsv
                  ortho <- read_tsv("C:/Users/lenovo/Desktop/WheatNLR/Data/1_thesis_data/1_yield/Orthofinder_results/Results_Apr24_5/Orthogroups/Orthogroups.tsv")
                  
                  # Select species of interest
                  selected_species <- c("CS_refseqv2", "Abo", "AMN", "ZM366", "Ae.speltoides", "Ae.tauschii", "T.turgidum", "T.urartu_2.1")  # Replace with actual column names
                  filtered <- ortho %>%
                    select(Orthogroup, all_of(selected_species))
                  
                  # Convert to binary presence/absence matrix
                  binary_matrix <- filtered %>%
                    mutate(across(-Orthogroup, ~ as.integer(!is.na(.) & . != ""))) %>%
                    column_to_rownames("Orthogroup")
                  
                  
                  
                  custom_colors <- c(
                    "CS_refseqv2"    = "#1f78b4",  # Blue
                    "Abo"            = "#e41a1c",  # Red
                    "AMN"            = "#ff7f00",  # Orange
                    "ZM366"          = "#f781bf",  # Pink
                    "Ae.speltoides"  = "#984ea3",  # Purple
                    "Ae.tauschii"    = "#4daf4a",  # Green
                    "T.turgidum"     = "#377eb8",  # Light Blue
                    "T.urartu_2.1"   = "#a65628"   # Brown
                  )
                  
                  # Enhanced UpSet plot
                  upset(
                    binary_matrix,
                    nsets = length(selected_species),
                    sets = selected_species,
                    sets.bar.color = custom_colors[selected_species],
                    main.bar.color = "black",  # Dark gray
                    matrix.color = "blue",     # Dot color
                    order.by = "freq",
                    mainbar.y.label = "Orthogroup Intersections",
                    sets.x.label = "Orthogroups per Species",
                    text.scale = 1.5              # Increase font size
                  )


```

```

 install.packages("VennDiagram")
                  library(VennDiagram)

                  
                  
                  # Choose species for Venn
                  venn_species <- c("CS_refseqv2", "Ae.speltoides", "Ae.tauschii", "T.urartu_2.1", "T.turgidum")
                  
                  # Build a list of orthogroups per species
                  venn_list <- lapply(venn_species, function(sp) {
                    ortho %>%
                      filter(!is.na(.data[[sp]]) & .data[[sp]] != "") %>%
                      pull(Orthogroup)
                  })
                  names(venn_list) <- venn_species                  
                  
                  
                  
                  
                  # Set custom colors for Venn diagram
                  venn_colors <- c("#1f78b4", "#984ea3", "#4daf4a", "#a65628", "#f781bf")
                  
                  # Generate Venn
                  venn.plot <- venn.diagram(
                    x = venn_list,
                    category.names = names(venn_list),
                    filename = NULL,
                    fill = venn_colors,
                    alpha = 0.6,
                    cex = 1.4,
                    cat.cex = 1.4,
                    fontface = "bold",
                    cat.fontface = "bold",
                    margin = 0.1
                  )
                  
                  # Plot to graphics device
                  grid.newpage()
                  grid.draw(venn.plot)

```
















                  
                  
