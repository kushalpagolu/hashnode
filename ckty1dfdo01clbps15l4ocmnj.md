## PEGA Security: RBAC

# Role-Based Access Control in PEGA.

## What is access control?

### Class, repeat after me.

Access control means ............ having control over access"(Just Kidding :P).

Let's take an example from a regular IT job. 

You are an IT employee(sadly) and you have an ID card or an Access card that you hang over your neck to show that you have a job for your neighbors.

But this **ID/Access** card has another important purpose for which it is designed.

It gives **access** to your **Facility**, your building, your bay, all the conference rooms, and war rooms, etc.

So, that is how Building/Facility **security** is enforced. **Access control **systems are often installed at building entrances, gates, interior doors, and elevators to ensure that only **authorized** people may **access** those spaces.

Now the same id card may not open all the doors in the building. Wait for a e-mail from HR when you swipe that card at any *Restricted Bay* or *High-Security* bays like server rooms etc. 

Building/Facility access control systems enforce security in 3 ways. One of the ways is by using **RBAC**.

**Role-based (RBAC): **In a role-based access control system, users are assigned a **role**, and **permissions** are granted according to those roles. Building **administrators** have the power to **assign** roles and **manage** access for each role.

So in the Restricted Bay scenario, your id card may not have a Maintenance/Admin role which has triggered the email from your HR :)

Well, **COVID-19** came along and said bye-bye to all this temporarily.


### So now let's get back to our topic in **PEGA** and try and relate security concepts. 

**PEGA** gives you an Enterprise Software, a Web Application, which can be deployed on-premise, Cloud, on a Docker, and other multi-tenant application servers. 

> *Consider this analogy for easy understanding - Consider your Pega Application as an IT Facility.*

Your **Pega application** is the kinda facility you work in. A typical **IT facility** has a Lobby, Project bay, HR bay, Maintenance, Server rooms, Conferences, Breakouts, and Restricted rooms.

In Pega, you need access to rules, data objects, system settings, and other features to build your application and also create work objects. By creating **operators** you provide access for users and processes to your application so that processing work(work object or a case id) can happen, and so that business processes can reach a successful resolution. 

As with any technology, you **Authenticate** your identity with a Login Id and Password to log into the application. 

In Pega, we call them **Operator Ids **(meh...). From our analogy this is your **ID card** to enter the Pega application.


It has a bunch of properties and is an instance of **Data-Admin-Operator-ID** and the data of the instances are saved to table **pr_operators** in **PegaDATA** database. After an operator logs in, the operator ID identifier is available on the **pxRequestor** page as the **pxUserIdentifier** property.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632100432128/NTgYTFP3k.png)

> The characteristics of an operator.

Now let's see how your operator id logs into any Pega application.

Once you hit the PRWeb URL of the Pega instance, you are presented with a login screen. Enter the operator id and password and log in. Most of the clients prefer External Identity providers like SAML, OAuth, AWS, and other alternatives for seamless authentication across all the applications the agent uses to process the work. (Out of Scope)

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632122139902/XRs0IVE3m.png)


A simple Pega login screen. You can always customize and specialize the screen according to client requirements. (Out of Scope)


Creating operators gives you the following benefits:
- Increased security.
- Improved routing of work.
- Relevant access type.


#### Now that we are clear with the basics, let's dig deep into our topic  

## Role-based access control (RBAC). 

**RBAC** is an **access-control** model based on organizing users into **roles** and assigning **permissions** to each role as appropriate. With RBAC, you can create a role for CSR members who are authorized to access Tier 1 case information and grant that role permission to authorize to view and modify Tier 1 cases. 

The Pega Platform implementation of role-based access control is based on two factors: **authentication** and **authorization**.

- **Authentication** confirms the identity of the user by validating login credentials such as the user name and password. In the Pega Platform, the **operator ID record** contains information needed to authenticate a user.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632364594781/v2rvZy2i4.png)

- **Authorization** determines the applications the user can access, including actions that the user can perform and information that the user can view. 

When a user signs in, Pega Platform identifies the **default access group** for the user and opens the **corresponding application** in the **specified portal**. 
A user can belong to multiple access groups, but only one access group is active at a time. 

RBAC is used to specify the access control requirements that pertain to the persona (user role) when using a Pega application.

Typically you implement a new application on top of the Pega application. When you create an application, 3 **roles** are created by default: **Admin**, **User**, and **Manager** and sometimes a 4th - **Author**. You can also add new roles based on client requirements. They perform the relevant actions assigned to them in the application created.

> Example : 

- "Dallas Nageshwar Rao" is a CSR using the Customer Service application, needing the authorization to create Service cases but is unauthorized to perform account changes for VIP customers.

- "Chicago Subba Rao" is a "Senior Account Manager" when using the Customer Service application, and is granted the authorization to perform account changes for VIP customers.

- Dallas, Nageshwar Rao, and Chicago Subba Rao's organizational roles determine what they are each **authorized** to do in the Customer Service application. 

**RBAC **can **restrict** the **operator** from accessing specific UI components, such as audit trails and attachments, or restrict the operator from performing specific **actions** on a case using privileges. You can also use RBAC to limit access to rules and application tools, such as Tracer and Access Manager during design time. Pega applications typically bring many personas (user roles) together to get work done. Roles include Customers, Call Center Workers, Managers, and Administrators. Most users fulfill one of the roles applicable for a given Application.

**RBAC** defines the actions that one role is **authorized** to perform on a class-by-class basis, including:

- Class-wide actions: Execute Activities, Run Reports.
- Record-level actions: Open, Modify (Create and Update), Delete. 
- Rule-specific actions as governed by Privileges.


An **access group** is associated with a user through the **Operator ID **record. You can open the operator id record from Dev Studio and find the Access Group it is assigned to.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632116448295/A-0MAdb5e.png)

# Let's look into RBAC and its rules in PEGA.

### Access Group 

An **Access Group** record is the starting point for the RBAC access control model. 
In Pega Platform, the access group record lists any authorized applications and roles assigned to members of the access group.

- A user can belong to multiple access groups, but only one access group is active at a time.

- Each user belongs to at least one access group. 

- An access group identifies the available application, default portal, and one or more assigned access roles for a group of users.
 

![Screen Shot 2021-09-20 at 3.34.42 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632135101119/fbfBSicI2.png)

Each access group references one or more **Access Role Name** records or roles.
 
An **access role name record** identifies the name of an access role and aggregates all of the individual access of role to object**(R-A-R-O)** and access deny records for that role. No access control configuration occurs on the access role name record itself.
 
* The most permissive access control setting is applied to all members of the access group.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632116362960/0dtgfBa_9.png)



### Access Role 

An access **ROLE** categorizes users according to the functions they perform in their work. Each **access role** represents how a set of users interacts with an application to create and process cases.

* Eg -Author, Manager, etc.


![Screen Shot 2021-09-20 at 5.10.18 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632135174554/xqvdH73j7.png)

- A role naming convention goes < ApplicationName >:Manager.

> Example

- An access role might represent the actions that a fulfillment operator, worker, or auditor is authorized to perform. A given operator can hold multiple access roles at one time. 

- It is a collection of **Access of Role to Object** and **Access Deny** rules defining all role-based access control settings that govern what access is granted (and denied) to those users who are allocated the access role. 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632101065845/R60Q7W873.png)

> Example

In a **health care application**, patients and doctors must be able to perform different tasks. Patients can check records and schedule appointments. Doctors can check patient records, record prescriptions, and add follow-up comments for patients after office visits. 

- Configure different **roles** for **patients** and **doctors** so that each user with the appropriate **role** specified can see the correct UI and have access to the desired application features.

**Standard access roles**

By default, Access Role Name records reference at least one standard access role as a dependent role. 

> Example 

The < ApplicationName >:Author role created for an application is based on the standard **PegaRULES:SysAdm4 **role, which lists the default access control settings for application developers. 

Now let's see the record of **BOFA:User4** role which was created using the new application wizard for the Cosmos application.


![Screen Shot 2021-09-21 at 3.09.18 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632210433719/f-akMwHwg.png)

As we discussed earlier, the access role is a collection of **Access of Role to Object** and **Access Deny** rules. It should have rules of ARO's in the table, but this record does not have any definition. So how does Pega provide access control to this role?

* Ans is **Dependent Roles.***

I will discuss more about **dependent roles** in the ***What's New in Pega 8?*** section at the bottom of this post.

### **Access of Role to Object (ARO) - **

An Access of Role to Object (**ARO**) record applies the access control configuration for instances of a class to members of a specific role. Each combination of class and role requires a unique ARO record.
 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631841111469/lNog96hhA.png)


![ARODesign.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1632127957452/B0kPAw12J.jpeg)

- Create an ARO for class.

- Configure the ARO at the appropriate level in the class hierarchy to achieve the desired effect.

- You use an ARO record to define the access control settings for instances of a specific class. 

- Each ARO identifies the actions that a role can perform on instances of the specified class.

- Each row in the definition is also called a **Rule to Access of Role to Object** (RARO).
 
> Example

- If the user story states that Auditors must have view-only access to case types and their child cases in the **SC-Dare-ViewStatement-Internet** and **SC-Dare-ViewStatement-Rovi** classes, you configure the **ARO** on the SC-Dare-ViewStatement class.
  
- The application has two child classes with the same configuration need. You make the configuration on the class of the **view statement** that applies to both types of services without affecting the other classes. 

![Screen Shot 2021-09-20 at 5.10.18 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632129133147/HuJf9BWIq.png)

With an **ARO**, permissions are granted on a 0-5 scale, where 0 denies permission to perform an action. The values 1-5 grant permission on a system with the same or a lower production level, as indicated in the following list.
 
0 No access

1 Experimental/Sandbox 

2 Development

3 Test/Quality assurance

4 Pre-Production/Staging

5 Production
 
Entering a permission setting of **5 **allows a user to view a case on a production system or any system with a lower production level setting. Entering a permission setting of 3 allows a user to view a case on a development or testing system, but not a staging or production system.


### **Access Deny(RDO)**
 
In certain situations, regulations and policies require explicit denial of access to specific capabilities. You use an Access Deny record to explicitly deny access to an action for instances of a class.
 
> Example

A data privacy requirement may state that users assigned the User role are denied access to PII. To satisfy this requirement, stakeholders decide to deny the ability to run reports on purchase requests to the User role. 

You can configure an **access deny record** to deny the **User** role the ability to run reports on instances of the Purchase Request case type.
 
Access deny records are functionally similar to AROs. As with an ARO, each **access deny** record corresponds to a unique combination of **role** and **class**. 




![Screen Shot 2021-09-21 at 2.16.26 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632205991105/gdC458eMoN.png)

With an access deny record, you deny permission for an action using a 0-5 scale, where 0 indicates that the action is allowed. The values 1-5 indicate that the action is denied on a system of the same or a higher production level.

Use an **Access Deny ** rule to explicitly deny authorization before evaluating whether any corresponding AROs for the same Class and Access Role may grant access to the same action. 

Note: If ARO and RDO records are defined for the same combination of role and class, the settings on the Access Deny record override the settings on the ARO.

 

*Rule resolution does not apply to **Access Deny** rules. Your system can contain at most **one** Access Deny rule for each **Applies To class** and Role Name combination.*

> Example

1. To explicitly prevent users from deleting cases you can configure an Access Deny record.

2. You configure an Access Deny record if the government or corporate regulations require explicitly denying an action to users, like viewing sensitive or highly confidential like credit card information of the customer.
 
### **Privilege**

To allow or deny access to certain rules, such as flow actions and correspondence, you configure a Privilege. For example, you add a privilege to a flow action used to approve purchase requests. Only roles that have been granted the privilege are allowed to approve a purchase request. If a user attempts to perform the action without the privilege, the application returns an error.
 

![Screen Shot 2021-09-20 at 6.13.03 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632135281703/EXbnlosdz.png)
Privileges are set on individual rules, such as flow actions. If a privilege is granted to an access role, the users assigned with that role can use the corresponding rule during their user session, such as during the processing of a case.
 
> Example:

1. Only holders of an ApprovePR Privilege can perform the Approve Purchase Request flow action.
2. Only holders of a ShowPR Privilege can see a Show Purchase Requests menu item configured within a Navigation rule.

Privileges provide better security because they can be used to control access to individual rules or parts of individual rules.


### Access When Rule

The rule defining conditional access control (like a regular When rule) typically evaluates one or more properties on the Class subject to authorization. 

The access control outcome yielded from the Access When rule varies between different instances of the same class.

![Screen Shot 2021-09-20 at 6.10.32 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632135261172/YsYm05nrK.png)

When rule varies between different instances of the same class.

> Example: 

1. Is the Service Request case currently in the Approval stage?
2. Is the Employee’s salary greater than USD-75000?




### Examples of RBAC
 
1. Consider a process for employee review that includes an assignment to authorize a discount for their customers. The assignment is routed to a common work queue that is accessible by all members of the **Manager's** department. Due to privacy concerns, stakeholders want to restrict access to raise proposals to only members of the Managers department that are authorized to access discount information. By granting authorization to specific members of the manager's department, you can reduce the chance of unauthorized access of managers who do not have the authority to approve discounts.

2. You can restrict an end user's ability to **perform** certain actions on a particular instance of a class at run time. 

3. You can limit a business or system architect's ability to **create**, **update**, or **delete** **rules** at design time or determine access to certain application development tools such as the **Clipboard** tool or the **Tracer** tool.

4. Service Agents at a Tesla call center belong to two access groups that inherit permissions from different dependent roles: Service Agents inherit permissions from the PegaRULES: User1, and Service Managers inherit from PegaRULES: WorkMgr4.

- Application requirements state that Service Agents and Managers can view data in Tesla-Service-Data-Painting and Tesla-Service-Data-Repair. 

- Only the Service Managers can delete data from Tesla-Service-Data-Painting and Tesla-Service-Data-Repair.

**Approach -**

> Configure an Access of Role to Object (ARO) record in Tesla-Service-Data- class to include the Service Manager's delete permissions. 

This approach is modular and maximizes the reuse of dependent role permissions. Instances of Tesla-Service-Data-Painting and Tesla-Service-Data-Repair inherit the permissions set in Tesla-Service-Data-. 

## What's New in Pega 8?

1. **Dependent Roles**

Although dependent rules existed before in Pega 7 its implementation in 8 is a little intrinsic. Pega allows you to define a **dependency hierarchy** of access roles that allows you to customize access control for use cases that require specific permissions. The concept behind a role dependency is that instead of a client “cloning” a role and then customizing it, the client creates a new role that is based on – dependent on – a Pega role which is shipped with the product. 

> The ***standard access role*** from which permissions are inherited is called a ***dependent role***. 

In the above example of **BOFA:User4** role, it has a dependent role configured out-of-the-box. That is the reason it does not have any **ARO's **configured in its record.


![Screen Shot 2021-09-21 at 3.46.45 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632212985583/gVNDyS8Rd.png)

This figure shows the second step toward adding a Dependent Role to just-created Access Role **BOFA:User4**. The Pega Platform-provided **PegaRULES:User4** is defined as a **Dependent role** in this record.

**PegaRULES:User4** - A standard role given by the Pega platform where Users with broader capabilities may open any work object in the application but perform assignments only on their worklists.



## Conclusion 

**Security** is a dry topic. A lot of content to go through and understand. The best way is to remember it with visual memory. Use the tree structures available in the images from the above sections. Try to picture how an Access Group and each of the rules are defined down the hierarchy. Once you get the idea, you can pretty much clear any **security-related** questions in tech rounds.

**Security** is designed at the very beginning of the App development and most of the operators and their corresponding accesses are decided upfront. Pega gives you 3,4 roles based on the application and built-on application configured while using the New Application wizard.

That is the reason we developers usually tend to ignore the nuances of **security** in Pega.

But if you actually ponder upon it, it is like a math puzzle, with all the permutations of **Roles** and combinations of **ARO's, RDO's** and **Privileges** combined with **Access When** and **Access Deny rules**. You can achieve your user story requirement in many ways. But, keep in mind, the solution has to be modular, reusable, effective, and most importantly achieve access control.

It is like Pega goes through a **Decision table** rule with all the combinations of **RBAC** configurations to identify the relevant role and provide only the access needed for users to perform work.

Growing business needs and security standards demand access to highly secure application access and robust data management. To meet client expectations, a system architect or a senior system architect must be able to understand the security requirements of the organization and configure the security based on the needs.


Source - https://pega.academy.com


For the interviews -

Actions that can be performed with RBAC and ABAC.

![Screen Shot 2021-09-19 at 8.46.38 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1632202940059/NMJCIW7VA.png)


P.S - Dedicated to the LSA who asked me a great **Security **related question in an **Interview**.