
# The Data Modeling Manifesto

1. ### **Form Follows Function** - Business goals come first[^1]; Data model will forever be derived from (data) user needs:
   1. **(User) business goals** drive **decisions** that need making.
   1. The decision-making needs drive which **(user) questions** need answering.
   1. The questions you need answered drive which **data** is needed (by the user) in order to answer them, and in which form. _This is your data model_.

1. ### Data models are expensive to maintain, so: **Reduce, Reuse, and Merge**
   1.  **Reduce** the number of unique models sent downstream, it would make life simpler for your consumers. Don't dump everything downstream, it creates usability, cost, and security issues. However, if is indeed required that additional data is to be sent, stored, and consumed, ship it, name it, tag it and document it in a manner that enables consumer to understand what it is that they are consuming, how if and when should they process the data, and how does each datapoint relate to other datapoints.
   1.  **Reuse** and extend common models, names, and tags instead of adding new ones whenever possible, this will minimize company-wide model span (so it is less complex overall) and will make sure your change will reach all relevant consumers.
   1.  **Merge** atomic data points to reflect their context -Data that is sent downstream should reflect context by consolidating several atomic data points to form a more complete picture. Grouping and aggregating data into sets, objects, lists, lines, documents, events, tables, hierarchies, or even files based on some heuristic has intrinsic value and can help consumers make sense of complex models and processes by analyzing the data that is relevant to them in its proper context.

1. ### **Metadata** should not be neglected or forgotten.
   1. **Everything has its time and place** - data is a transient thing, when sending data, it is true from a certain perspective (the senders') and to some point in time (when was it recorded? When was it received? How much can you trust these timestamps?). Make sure to note both.
   1.  " **Handle with care" tags are mandatory** - some data requires special treatment, whether it is _PII, financial, IDs, index keys,_ or some other unique case, tagging data as such would help prevent consumers from mishandling it.
   1. **Sound body, sound mind** - Define health metrics and smells for your data: a revenue field in a purchase event may be expected to forever contain positive numbers, a counter field may be expected to monotonically rise, etc.

1. ### **Interface over Implementation**
   1. **Meaning is more important than implementation** - Data is a means for representing information, and that information should have a meaning. When representing information, the data types we choose can be extremely specific or extremely vague, but our business needs are exact. Prefer meaningful types and names (use your organization's [ubiquitous language](https://www.oreilly.com/library/view/domain-driven-design-tackling/0321125215/)!) over those that reflect internal implementation - Count is better than Integer which in turn is better than Number, UUID is better than ID which is of course better than String, Sets and Ordered Lists are better than Arrays, etc.[^2].
   1. Make sure to **design your model in tandem** with the intended (initial) user. _DO_ suit their business goals and needs, _DON'T_ accommodate for your internal implementation constraints.
   1. **Design it to be changed** - You have no idea what will have to change in the (near OR far) future, make sure to accommodate for that (use versioning, optional fields, etc.) while keeping in mind that some things are inherent to the model and should never change (these are usually your 'required' fields).

1. ### Data, models, systems, and **data modeling systems**
   1. As data is representative of facts (as far as the source can tell) and models are derived from business needs, **no data, models, or data models should be affected by changes in the data transport system(s), their design or implementation**. Field names and values, event names, the result of a measurement, or a user identifier will all remain the same regardless of the system that is responsible for reporting it.
   1. To keep track of your models, **a bookkeeping system (glossary, registry) must be in place, as well as the processes around it**. Defining roles and responsibilities around the creation, storage and retrieval of models is key to keeping data velocity in producing and consuming teams high.
   1. Storage is cheap, compute is expensive - **store partial results and aggregations in a way that would benefit future reference and use**. This would lower cost of ownership and reduce model divergence, especially when managing complex model structures and analysis. The set of actions performed by a user during their 1st session using your product may be used, for example, for _LTV prediction_ as well as _popular feature analysis_, and _churn/friction analysis_, separating aggregation from analysis would help optimize your pipeline for any use case and business need that may arise in the future.


[^1]: Data is stored for a to serve a single purpose – to meet some business needs for some consumer. Whether that consumer (=data user) is internal to the company or is a client has no effect on the process – they still have some goal or need that should be met and require the same level of diligence in modeling data for their consumption and use.

[^2]: Leaving a field as String knowing sources will populate them with non-schema controlled (or worse yet, unmodelled) structured data (such as JSON, XML, dumps, etc.) through it should be avoided at all costs, as even the existence of these “path of least resistance” fields is tempting data source developers to bypass the desired modelling process. The risk posed by these fields, that there would exist some business-critical model that will not be tracked and managed as part of the normal flow of data in the company precludes any benefit one may derive from allowing such field to exist. Just like you would not allow any rouge source code to be added to a product without proper code review, version control and testing, so shouldn’t rouge data models exist in your product.
