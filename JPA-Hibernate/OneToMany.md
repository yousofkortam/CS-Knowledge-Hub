One to many relationships (@OneToMany) are characterized by Entity where we have a main object and collection of objects from another directly related Entity. 
These relationships are defined by collections, as we will have a series of objects belonging to the main object.

A classic example to understand this type of relationship is invoices, since we will have a leading Entity where we have the main data of the invoice, such as series, customer, total, date of issue, etc. On the other hand, the invoice will have a series of lines that represents each of the products sold.

Let's see how the Invoice entity (invoice) would look:

```java

import java.util.Calendar;
import java.util.List;
import javax.persistence.*;

@Entity
@Table(name="INVOICES")
public class Invoice {
    
    @Id
    @Column(name="ID")
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;
    
    @Column(name = "STATUS", length = 20, nullable = false)
    @Enumerated(EnumType.STRING)
    private Status status;
    
    @Temporal(TemporalType.TIMESTAMP)
    @Column(name = "REGIST_DATE", nullable = false)
    private Calendar registDate;
    
    @JoinColumn(name = "FK_CUSTOMER", nullable = false)
    @ManyToOne(optional = false, cascade = CascadeType.ALL, fetch = FetchType.EAGER)
    private Customer customer;
    
    @OneToOne(mappedBy = "invoice", cascade = CascadeType.ALL)
    private Payment payment;
    
    @OneToMany(cascade = CascadeType.ALL, mappedBy = "invoice")
    private List<InvoiceLine> lines;

    /** GET and SET */
}
```

We can see how we have defined the lines property as a list (List), which allows us to relate the invoice to an indeterminate number of lines, on the other hand, we have defined the mappedBy property to indicate that it is a bidirectional relationship, that is, the InvoiceLine Entity will also have a relationship towards the Invoice Entity.

The InvoiceLine Entity will look like this:

```java
import javax.persistence.*;

@Entity
@Table(name = "invoice_lines")
public class InvoiceLine {
    @Id
    @Column(name = "ID")
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "PRODUCT", nullable = false)
    private String product;
    
    @Column(name = "PRICE", nullable = false)
    private double price;
    
    @Column(name = "TOTAL", nullable = false)
    private double quantity;
    
    @ManyToOne
    @JoinColumn(name = "FK_INVOICE", nullable = false, updatable = false)
    private Invoice invoice;
    
    /** GET and SET */

}
```

As we said a moment ago, the InvoiceLine Entity has an Invoice type property to be able to make it bidirectional, let's observe that the name of the invoice property corresponds to the value of the mappedBy defined in the Invoice Entity, since otherwise JPA will throw us an error .

We can see that the invoice_lines table has the FK_INVOICE column that will be used to perform the JOIN with the invoices table, in such a way that each invoice_lines record that is found will be converted into an instance of the InvoiceLine Entity.

```java
public static void main(String[] args) {
    Invoice invoice = new Invoice();

    List<InvoiceLine> lines = new ArrayList<>();
    for(int c = 0; c<10; c++){
        InvoiceLine line = new InvoiceLine();
        line.setInvoice(invoice);
        line.setPrice(10);
        line.setProduct("Product " + (c+1));
        line.setQuantity(c+1);
        lines.add(line);
    }

    Customer customer = new Customer();
    customer.setName("Oscar Blancarte");

    invoice.setCustomer(customer);
    invoice.setLines(lines);
    invoice.setRegistDate(Calendar.getInstance());
    invoice.setStatus(Status.ACTIVE);

    EntityManager em = EntityManagerUtil.getEntityManager();
    em.getTransaction().begin();
    em.persist(invoice);
    em.getTransaction().commit();
}
```
    private double price;
