# eco-commute-tracker

Creating an eco-commute tracker involves implementing a Python program that allows users to track their commutes, calculate their carbon footprint, and find ways to optimize for more eco-friendly alternatives. Below is a complete Python program that achieves this:

```python
class CommuteOption:
    def __init__(self, name, carbon_emission_per_km, cost_per_km):
        self.name = name
        self.carbon_emission_per_km = carbon_emission_per_km  # in kg CO2
        self.cost_per_km = cost_per_km  # in currency unit

    def calculate_footprint(self, distance):
        """Calculate total carbon emissions for a given distance."""
        return self.carbon_emission_per_km * distance

    def calculate_cost(self, distance):
        """Calculate total cost for a given distance."""
        return self.cost_per_km * distance

class CommuteTracker:
    def __init__(self):
        self.options = {}
        self.journals = []

    def add_option(self, name, carbon_emission_per_km, cost_per_km):
        """Add a new commuting option."""
        self.options[name] = CommuteOption(name, carbon_emission_per_km, cost_per_km)

    def add_journal(self, option_name, distance):
        """Record a commute with the given option and distance."""
        if option_name not in self.options:
            raise ValueError("Commute option not recognized. Please add it first.")
        self.journals.append((option_name, distance))

    def summarize_commutes(self):
        """Summarize total commute metrics."""
        total_emissions = 0
        total_cost = 0
        for option_name, distance in self.journals:
            option = self.options[option_name]
            total_emissions += option.calculate_footprint(distance)
            total_cost += option.calculate_cost(distance)
        
        return total_emissions, total_cost

    def suggest_eco_friendlier_option(self, desired_emission_reduction):
        """Suggest a commuting option to meet a desired emission reduction."""
        if not self.journals:
            return "No commute records to analyze."
        
        original_emissions, _ = self.summarize_commutes()
        target_emissions = original_emissions - desired_emission_reduction
        
        if target_emissions < 0:
            return "Your desired reduction exceeds current total emissions."
        
        sorted_options = sorted(self.options.values(), key=lambda o: o.carbon_emission_per_km)
        for option in sorted_options:
            if option.calculate_footprint(1) * len(self.journals) <= target_emissions:
                return f"Consider using {option.name} to reduce emissions."
        return "No suitable alternative found to meet desired reduction."

# Example usage of the Eco-Commute Tracker
def main():
    tracker = CommuteTracker()
    
    # Adding commute options
    tracker.add_option("Car", 0.21, 0.15)
    tracker.add_option("Bicycle", 0.0, 0.02)
    tracker.add_option("Bus", 0.05, 0.10)
    tracker.add_option("Walk", 0.0, 0.0)
    
    # Logging some commute data
    try:
        tracker.add_journal("Car", 10)
        tracker.add_journal("Bus", 5)
        tracker.add_journal("Bicycle", 3)
        tracker.add_journal("Walk", 2)
    except ValueError as e:
        print(f"Error: {e}")

    # Summarizing total emissions and costs
    emissions, cost = tracker.summarize_commutes()
    print(f"Total emissions: {emissions:.2f} kg CO2, Total cost: ${cost:.2f}")

    # Suggesting eco-friendly option
    suggestion = tracker.suggest_eco_friendlier_option(1.0)  # Assuming a desire to cut 1 kg CO2
    print(suggestion)

if __name__ == "__main__":
    main()
```

### Explanation:

- **CommuteOption class**: Represents a mode of commuting with associated carbon emissions and costs per kilometer. It provides methods to calculate total emissions and costs for a given trip distance.

- **CommuteTracker class**: Manages multiple commuting options and provides functionalities to log commute journals, summarize total emissions and costs, and suggest more eco-friendly alternatives.

- **Error Handling**: Basic error handling is implemented in `add_journal` to handle cases where a non-existent commute option is logged.

- **Example Usage**: The `main` function demonstrates how to use the `CommuteTracker` to add commute options, log commutes, and carry out analyses.