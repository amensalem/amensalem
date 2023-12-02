q1)


// Order.cs
public class Order
{
    public int Id { get; set; }
    public Customer Customer { get; set; }
    public DateTime Date { get; set; }
    public string Status { get; set; }
    public List<OrderDetail> OrderDetails { get; set; }

    public decimal CalcSubTotal()
    {
        decimal subTotal = 0;
        foreach (OrderDetail orderDetail in OrderDetails)
        {
            subTotal += orderDetail.CalcSubTotal();
        }
        return subTotal;
    }

    public decimal CalcTax()
    {
        decimal tax = 0;
        foreach (OrderDetail orderDetail in OrderDetails)
        {
            tax += orderDetail.CalcTax();
        }
        return tax;
    }

    public decimal CalcTotal()
    {
        return CalcSubTotal() + CalcTax();
    }

    public decimal CalcTotalWeight()
    {
        decimal totalWeight = 0;
        foreach (OrderDetail orderDetail in OrderDetails)
        {
            totalWeight += orderDetail.CalcWeight();
        }
        return totalWeight;
    }
}

// OrderDetail.cs
public class OrderDetail
{
    public int Id { get; set; }
    public Item Item { get; set; }
    public int Quantity { get; set; }
    public decimal Price { get; set; }
    public string TaxStatus { get; set; }

    public decimal CalcSubTotal()
    {
        return Quantity * Price;
    }

    public decimal CalcTax()
    {
        decimal taxRate = 0.06m;
        if (TaxStatus == "Tax Exempt")
        {
            return 0;
        }
        else
        {
            return CalcSubTotal() * taxRate;
        }
    }

    public decimal CalcWeight()
    {
        return Quantity * Item.Weight;
    }

    public bool InStock()
    {
        return Item.QuantityInStock >= Quantity;
    }
}

// Item.cs
public class Item
{
    public int Id { get; set; }
    public string Description { get; set; }
    public decimal Price { get; set; }
    public decimal Weight { get; set; }
    public int QuantityInStock { get; set; }
}

// Payment.cs
public abstract class Payment
{
    public float Amount { get; set; }

    public abstract bool Authorized();
}

// Cash.cs
public class Cash : Payment
{
    public float CashTendered { get; set; }

    public override bool Authorized()
    {
        return CashTendered >= Amount;
    }
}

// Check.cs
public class Check : Payment
{
    public string Name { get; set; }
    public string BankId { get; set; }

    public override bool Authorized()
    {
        // TODO: Implement check authorization logic here.
        return true;
    }
}

// Credit.cs
public class Credit : Payment
{
    public string Name { get; set; }
    public string Type { get; set; }
    public DateTime ExpDate { get; set; }

    public override bool Authorized()
    {
        // TODO: Implement credit card authorization logic here.
        return true;
    }
}
