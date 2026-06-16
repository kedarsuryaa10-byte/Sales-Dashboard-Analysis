# Sales-Dashboard-Analysis
Sales Dashboard Analysis – Project Description  The Sales Dashboard Analysis project focuses on transforming raw sales data into meaningful insights using data visualization and analytical tools. The objective of this project is to track business performance, identify trends, and support data-driven decision-making
    self.prepare_data()

def generate_sample_data(self):
    """Generate sample sales data"""
    np.random.seed(42)
    dates = pd.date_range(start='2024-01-01', periods=500, freq='D')
    
    data = {
        'date': np.repeat(dates, 5),
        'product': np.tile(['Product A', 'Product B', 'Product C', 'Product D', 'Product E'], 500),
        'region': np.tile(np.repeat(['North', 'South', 'East', 'West', 'Central'], 1), 500),
        'customer_id': np.random.randint(1000, 9999, 2500),
        'revenue': np.random.uniform(500, 5000, 2500),
        'cost': np.random.uniform(200, 2500, 2500),
        'quantity': np.random.randint(1, 20, 2500),
    }
    
    df = pd.DataFrame(data)
    df['profit'] = df['revenue'] - df['cost']
    df['date'] = pd.to_datetime(df['date'])
    return df

def prepare_data(self):
    """Prepare and clean data"""
    self.df['date'] = pd.to_datetime(self.df['date'])
    self.df['month'] = self.df['date'].dt.to_period('M')
    self.df['week'] = self.df['date'].dt.to_period('W')
    self.df['profit'] = self.df['revenue'] - self.df['cost']

def get_revenue_summary(self):
    """Get revenue KPIs"""
    return {
        'total_revenue': self.df['revenue'].sum(),
        'avg_revenue': self.df['revenue'].mean(),
        'max_revenue': self.df['revenue'].max(),
        'min_revenue': self.df['revenue'].min(),
    }

def get_profit_summary(self):
    """Get profit KPIs"""
    return {
        'total_profit': self.df['profit'].sum(),
        'avg_profit': self.df['profit'].mean(),
        'profit_margin': (self.df['profit'].sum() / self.df['revenue'].sum()) * 100,
    }

def revenue_by_product(self):
    """Revenue breakdown by product"""
    return self.df.groupby('product')['revenue'].sum().sort_values(ascending=False)

def revenue_by_region(self):
    """Revenue breakdown by region"""
    return self.df.groupby('region')['revenue'].sum().sort_values(ascending=False)

def profit_by_region(self):
    """Profit breakdown by region"""
    return self.df.groupby('region')['profit'].sum().sort_values(ascending=False)

def monthly_trend(self):
    """Monthly revenue and profit trend"""
    monthly = self.df.groupby('month').agg({
        'revenue': 'sum',
        'profit': 'sum',
        'quantity': 'sum'
    })
    return monthly

def customer_analysis(self):
    """Customer behavior analysis"""
    return self.df.groupby('customer_id').agg({
        'revenue': 'sum',
        'profit': 'sum',
        'quantity': 'sum',
        'product': 'nunique'
    }).rename(columns={'product': 'products_bought'})

def product_performance(self):
    """Product performance metrics"""
    return self.df.groupby('product').agg({
        'revenue': 'sum',
        'profit': 'sum',
        'quantity': 'sum',
        'customer_id': 'nunique'
    }).rename(columns={'customer_id': 'unique_customers'})

def plot_revenue_trend(self):
    """Plot revenue trend over time"""
    monthly = self.monthly_trend()
    plt.figure(figsize=(12, 6))
    plt.plot(monthly.index.astype(str), monthly['revenue'], marker='o', linewidth=2)
    plt.title('Monthly Revenue Trend')
    plt.xlabel('Month')
    plt.ylabel('Revenue ($)')
    plt.xticks(rotation=45)
    plt.grid(True, alpha=0.3)
    plt.tight_layout()
    return plt

def plot_regional_comparison(self):
    """Plot regional revenue comparison"""
    region_rev = self.revenue_by_region()
    plt.figure(figsize=(10, 6))
    region_rev.plot(kind='bar', color='skyblue')
    plt.title('Revenue by Region')
    plt.xlabel('Region')
    plt.ylabel('Revenue ($)')
    plt.xticks(rotation=45)
    plt.tight_layout()
    return plt

def plot_product_mix(self):
    """Plot product revenue mix"""
    product_rev = self.revenue_by_product()
    plt.figure(figsize=(10, 6))
    plt.pie(product_rev.values, labels=product_rev.index, autopct='%1.1f%%')
    plt.title('Revenue Distribution by Product')
    plt.tight_layout()
    return plt
