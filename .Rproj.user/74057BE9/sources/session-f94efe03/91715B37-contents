# CO2 Emissions Analysis: South Africa (1990-2024)

# Install required packages
packages <- c("readr", "dplyr", "ggplot2")

for (p in packages) {
  if (!requireNamespace(p, quietly = TRUE)) {
    install.packages(p)
  }
}

library(readr)
library(dplyr)
library(ggplot2)

# Create folders
dir.create("data", showWarnings = FALSE)
dir.create("figures", showWarnings = FALSE)
dir.create("results", showWarnings = FALSE)

# Download CO2 dataset
url <- "https://owid-public.owid.io/data/co2/owid-co2-data.csv"

co2_raw <- read_csv(url, show_col_types = FALSE)

# Select South Africa data
co2_data <- co2_raw %>%
  filter(
    iso_code == "ZAF",
    year >= 1990,
    year <= 2024
  ) %>%
  select(country, iso_code, year, co2, co2_per_capita) %>%
  arrange(year)

# Check data
if (nrow(co2_data) == 0) {
  stop("No South Africa data found.")
}

# Save filtered data
write_csv(co2_data, "data/south_africa_co2_1990_2024.csv")

# Calculate summary statistics
valid_co2 <- co2_data %>% filter(!is.na(co2))

first_year <- valid_co2 %>% slice_head(n = 1)
last_year <- valid_co2 %>% slice_tail(n = 1)
peak_emissions <- valid_co2 %>% slice_max(co2, n = 1)

percentage_change <- 
  ((last_year$co2 - first_year$co2) / first_year$co2) * 100

summary_results <- data.frame(
  Country = "South Africa",
  ISO_Code = "ZAF",
  Study_Period = "1990-2024",
  First_Valid_Year = first_year$year,
  Last_Valid_Year = last_year$year,
  First_Year_CO2 = first_year$co2,
  Last_Year_CO2 = last_year$co2,
  Percentage_Change = percentage_change,
  Peak_Year = peak_emissions$year,
  Peak_Emissions = peak_emissions$co2
)

# Save summary
write_csv(summary_results, "results/south_africa_summary.csv")

print(summary_results)

# Graph 1: Annual CO2 emissions
graph1 <- ggplot(valid_co2, aes(x = year, y = co2)) +
  geom_line(color = "blue") +
  geom_point(color = "blue") +
  labs(
    title = "South Africa - Annual CO2 Emissions",
    x = "Year",
    y = "CO2 emissions (million tonnes)"
  ) +
  theme_minimal()

ggsave("figures/annual_co2_emissions.png", graph1,
       width = 10, height = 6)

# Graph 2: CO2 emissions per capita
valid_per_capita <- co2_data %>%
  filter(!is.na(co2_per_capita))

graph2 <- ggplot(valid_per_capita,
                 aes(x = year, y = co2_per_capita)) +
  geom_line(color = "red") +
  geom_point(color = "red") +
  labs(
    title = "South Africa - CO2 Emissions Per Capita",
    x = "Year",
    y = "Tonnes of CO2 per person"
  ) +
  theme_minimal()

ggsave("figures/co2_per_capita.png", graph2,
       width = 10, height = 6)

# Display graphs
print(graph1)
print(graph2)