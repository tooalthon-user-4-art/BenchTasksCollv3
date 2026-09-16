You are a helpful AI assistant working on a task that implements a loyalty program for a retail store. The goal is to create a system that tracks customer purchases, calculates loyalty points, and provides rewards for customers based on their spending. The system should handle multiple customers, track purchases, and generate reports on loyalty program performance.

## Files to create:

### 1. loyalty_program.py
```python
# Loyalty Program Implementation
# This module implements a customer loyalty program with point tracking and rewards

class LoyaltyProgram:
    def __init__(self):
        self.customers = {}
        self.points_rate = 1  # 1 point per dollar spent
        self.points_threshold = 1000  # Points needed for a reward

    def add_customer(self, customer_id, name):
        """Add a new customer to the loyalty program"""
        if customer_id not in self.customers:
            self.customers[customer_id] = {
                'name': name,
                'points': 0,
                'total_spent': 0,
                'rewards_claimed': []
            }
            return True
        return False

    def record_purchase(self, customer_id, amount):
        """Record a purchase and award points"""
        if customer_id not in self.customers:
            return False
        points_earned = int(amount * self.points_rate)
        self.customers[customer_id]['points'] += points_earned
        self.customers[customer_id]['total_spent'] += amount
        return True

    def get_customer_status(self, customer_id):
        """Get current status of a customer"""
        if customer_id not in self.customers:
            return None
        return self.customers[customer_id]

    def calculate_rewards(self):
        """Calculate available rewards for all customers"""
        rewards = []
        for customer_id, customer in self.customers.items():
            if customer['points'] >= self.points_threshold:
                rewards_available = customer['points'] // self.points_threshold
                rewards.append({
                    'customer_id': customer_id,
                    'customer_name': customer['name'],
                    'rewards_available': rewards_available,
                    'points': customer['points']
                })
        return rewards

    def claim_reward(self, customer_id, reward_amount):
        """Claim a reward for a customer"""
        if customer_id not in self.customers:
            return False
        if self.customers[customer_id]['points'] >= self.points_threshold * reward_amount:
            self.customers[customer_id]['points'] -= self.points_threshold * reward_amount
            self.customers[customer_id]['rewards_claimed'].append(reward_amount)
            return True
        return False

    def generate_report(self):
        """Generate a summary report of the loyalty program"""
        total_customers = len(self.customers)
        total_points = sum(c['points'] for c in self.customers.values())
        total_spent = sum(c['total_spent'] for c in self.customers.values())
        customers_with_rewards = sum(1 for c in self.customers.values() if c['points'] >= self.points_threshold)
        
        return {
            'total_customers': total_customers,
            'total_points_awarded': total_points,
            'total_revenue': total_spent,
            'customers_eligible_for_rewards': customers_with_rewards,
            'average_points_per_customer': total_points / total_customers if total_customers > 0 else 0
        }

# Example usage
if __name__ == '__main__':
    program = LoyaltyProgram()
    program.add_customer('C001', 'Alice Johnson')
    program.add_customer('C002', 'Bob Smith')
    program.record_purchase('C001', 150.00)
    program.record_purchase('C001', 200.00)
    program.record_purchase('C002', 75.50)
    print(program.generate_report())
    print(f"Customer C001 status: {program.get_customer_status('C001')}")
    print(f"Available rewards: {program.calculate_rewards()}")
```