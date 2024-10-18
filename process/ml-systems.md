## Identify the problem

- Collaborate cross-functionally with stakeholders to understand business objectives and the problem we are trying to solve. We don’t want to contribute to a ML graveyard (unused ML models that sit in production for no reason).
- Are there OKRs that are high priority but far from target?

### Understanding Success metrics

- Define clear objectives.
- Each objective should align against company goals (increase engagement, increase revenue, etc).
- Evaluate objective against SMART framework.
- Clear, quantifiable metrics to measure the success of the feature or system beyond model performance. Examples include:
  - ROI
  - Lifetime value
  - Conversion rate

## Feasibility research

- What data, infrastructure, and tools are available to support the design of the new solution? Is the desired outcome achievable within the technical constraints?

### Data

- Focus of feasibility research is primarily on the data:
  - Relevance - Does data exist related to the problem we are trying to solve?
  - Availability - Do we collect the data internally or do we have to go get it? Can we get it in real time for inference?
  - Quality - Is the data accurate? Is it complete?
  - Quantity - Do we have enough of it?
  - Privacy - Are we allowed to use the data commercially? Does it have to be deidentified in any way?
  - Bias or Representativeness - Is the data representative of the population? Will it lead to biased predictions against certain subsets of the population?
  - Storage - If the data is not available, how will we store it?
  - Success Metrics & Target - Do we have the right data to compute our target? Do we have the right data to compute our success metrics?

### Infra

- We can lightly consider infra capabilities, but we will probably spend more time on this in later steps:
  - Compute - Where will we train the model?
    - Locally
    - Cloud - EC2, Sagemaker
  - Storage - Where will we store things like model artifacts, engineered data, and predictions?
    - Blob storage - S3
    - Database - DynamoDB
  - Scalability
    - How does data flow through the system? Does data need to be accessed in real time? Do predictions need to be generated in real time?
    - How do we expect the model to utilize compute? Do we need to parallelize tasks? Do we need to plan for the dynamic scaling of resources?
    - Cost - Are our expected costs going to outweigh the benefit?

## Prototype Development

### Model specification

- Is the problem a classification, regression, clustering, or time series problem?
- How big is our training dataset? How many features do we have? What level of preprocessing is required? How is the data distributed?
- How complex is the relationship between the features and our target?
- What level of explainability do we need?
- What level of compute do we have access to?
- Do we need real-time or batch inference? Do we need super fast predictions?
- Are there regulatory or legal ramifications stemming from the selection of certain models?
- Is cross-validation enough? Do we need a holdout set? Do we want the model to run in shadow mode to collect predictions on unseen data?

### Compute

- Where will we train the model?
  - EC2 - customizable and scalable infrastructure on demand.
  - Sagemaker - fully managed model training service.
- Storage - Where will we store things like model artifacts & docker containers?
  - S3 - scalable, fast blob storage.
  - RDS - relational databases if needed.
  - ECR - fully managed container registry.
- Consider using poetry to manage the virtual environment.
- Unit testing to ensure model is behaving as expected.

## Stakeholder Feedback

- Collect feedback from relevant stakeholders to gather feedback on potential impact and usability.
- Confirm model is helping to solve the problem originally identified.
- Is the model making systematic errors in its predictions?
- Iteratively improve the model to reflect initial feedback and suggestions to the extent we can.
- Potentially use AWS Ground Truth to give stakeholders a chance to relabel data.

## Production Planning

### Architecture design

- Inference pipeline - AWS lambda for serverless inference (possibly step functions as well).
- Data retrieval - Access raw data needed to generate new predictions.
- Data Preprocessing - Run raw data through preprocessing pipeline to ensure it’s in the format the model expects. Output engineered data to S3 indexed by some key.
- Model Inference - Invoke model endpoint using engineered data.
- Any post-process steps - e.g. converting numerical outputs to labels, saving predictions to S3.
- Deliver predictions to the application that requested them - via S3, an API endpoint, etc.

### Scalability

- Horizontal vs. Vertical Scaling: Add more machines (horizontal) or upgrade existing hardware (vertical) as needed – AWS lambda automatically manages infrastructure and scales to demand.
- Elasticity: Utilize cloud services that offer automatic scaling capabilities to adjust resources dynamically in response to the system’s load.
- Data partitioning: Distribute data across multiple systems to reduce the load on any single system.
- Load balancing: Distribute network traffic across multiple servers to avoid overwhelming one server - AWS ELB automatically distributes incoming traffic across multiple targets (such as lambda functions).

### Reliability

- Fault tolerance: Ensure the system continues to operate in the event one of its components fails.
  - Redundant systems - Replicate infrastructure across multiple regions.
  - Automatic failover mechanisms.
  - Reliable storage systems - S3 is 99.9999% durable.
  - ELB will distribute traffic for AWS resources in different availability zones.
  - Regular backup procedures & robust recovery plan.
  - Comprehensive monitoring and alerts to track the system's health and performance in real time - AWS Cloudwatch to identify and react to changes in AWS resources.

### Maintenance

- CICD to automate data preprocessing, model training, and deployment - CI/CD can be managed through GitHub.
- Version Control to track changes, manage dependencies, and ensure reproducibility of results - GitHub.
- Monitoring to identify data drift or model irregularities.

## DevOps

- IaC to manage and provision computing infra via machine-readable files?
- Containerization to encapsulate the application and its dependencies to ensure consistent environments across testing/production and simplify deployment and testing.
- Microservices - Do we want the system to be available as smaller, independently deployable services?
- Feedback loop - Create an automatic training pipeline to update data, retrain models, and deploy new models as new data is accumulated.

## Risk Assessment

- What are the major risks to the system? Data drift, performance bottlenecks, failure points.
- What do we do about the risks?
- What is the rollback plan in case the system fails?

## Implementation and Deployment

- To deploy our model:
  - Containerize the model - should contain everything needed to run the model, including code, runtime, libraries, etc.
  - Push container to AWS Elastic Container Registry (ECR).
  - Deploy inference pipeline:
    - AWS Lambda to run inference code if it’s within Lambda’s memory limits.
    - SageMaker endpoint if outside of Lambda limits.
    - Step functions to orchestrate multi-step inference pipeline if needed.
  - Ensure resources have access to the required resources.
  - Set up ELB if necessary (not necessary for SageMaker).
  - Set up AWS CloudWatch for monitoring of the application’s performance and health. Configure alarms.
  - Use CI/CD tools (CodePipeline, CodeBuild, GitHub Actions) to automate build, test, and deployment of containerized model.
  - Deploy to staging environment before going live. Use this as a chance to:
    - Run integration tests.
    - Load testing.
    - Consider deploying the feature or system to a staging environment (shadow mode) before pushing to production to ensure nothing breaks.
    - A/B testing to confirm value add or identify unexpected behavior before rolling out to everyone.

## Monitoring & Maintenance

- Ensure tools are in place to evaluate the system against our success metrics.
- Plan for regular model & system reviews.
- Establish feedback loop.
