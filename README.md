# GitHub_SLTest_01 — SnapLogic CI/CD Demo with GitHub Actions

This repository is a **demo/sandbox project** that shows how to manage **SnapLogic assets in Git** and promote changes using a **GitHub Actions CI/CD workflow** that calls the **SnapLogic Public APIs**.

The project includes example SnapLogic pipelines and a task, along with a GitHub Actions workflow that:

1. Creates a temporary test project space and project in SnapLogic
2. Checks out this GitHub repository into the test project
3. Migrates the assets to a staging project space
4. Applies permissions (ACL) in the staging environment

This repository is intended for **demonstration, testing, and learning** SnapLogic Git-based CI/CD patterns.

---

## Repository Structure
.
├── .github/workflows/
│ └── sl-workflow.yaml
├── ACB - Demo 01 Pipeline.slp
├── ACB - Git Version Control Example.slp
├── ACB - Git Version Control Example Task.slt
├── Client1/test.txt
├── Client2/test.txt
├── NewFile.txt
└── README.md

### Key Components

#### SnapLogic Pipelines

**ACB - Demo 01 Pipeline.slp**
- Oracle Select
- Mapper
- Salesforce Update  
Demonstrates a simple database-to-SaaS update pattern.

**ACB - Git Version Control Example.slp**
- Salesforce Read
- Mapper
- Copy (branching)
  - SQL Server Insert
  - CSV Formatter → File Writer  
Demonstrates a typical integration flow with branching outputs.

#### SnapLogic Task

**ACB - Git Version Control Example Task.slt**
- Pipeline Job definition
- Executes the `ACB - Git Version Control Example` pipeline
- Uses the `UltraCloudplex` Snaplex
- Configured as a scheduled task (no schedule defined in export)

---

## CI/CD Workflow

Workflow file:
.github/workflows/sl-workflow.yaml


Workflow name:
SnapLogic CI/CD Demo


### Triggers
The workflow runs:

- On push to `main`
- On pull requests targeting `main`
- Manually via `workflow_dispatch`

### Workflow Steps

1. **Create Test Project Space**
2. **Create Test Project**
   - Named using the GitHub run number
3. **Checkout Repository into SnapLogic**
4. **Migrate Assets to Staging**
5. **Wait for Migration Completion**
6. **Apply ACL Permissions to Staging Project**

---

## SnapLogic API Endpoints Used

The workflow calls SnapLogic public APIs at:
https://cdn.elastic.snaplogic.com/api/1/rest/public/


Primary endpoints:

- `assetapi/project/...`  
  Create project spaces and projects
- `project/checkout/...`  
  Clone Git repo into SnapLogic
- `project/migrate/...`  
  Promote assets between project spaces
- `assetapi/acl/...`  
  Apply permissions

---

## Required GitHub Secrets

Add these under:

**Repository → Settings → Secrets and variables → Actions**

| Secret Name        | Description |
|--------------------|-------------|
| `SL_USER`          | SnapLogic username (email) |
| `SL_PASSWORD_TEST` | SnapLogic password or CI credential |

These are used by the workflow as:
USER: ${{ secrets.SL_USER }}
PASS: ${{ secrets.SL_PASSWORD_TEST }}


---

## Workflow Environment Variables

Defined inside `sl-workflow.yaml`:

| Variable | Purpose | Example |
|----------|---------|---------|
| `URL` | SnapLogic API base URL | `https://cdn.elastic.snaplogic.com` |
| `ORG` | SnapLogic organization name | `ConnectFasterInc` |
| `PROJECT_SPACE` | Temporary test project space | `Clark_Test` |
| `PROJECT_SPACE_PROMOTE` | Staging project space | `Clark_Staging` |

Adjust these values to match your SnapLogic environment.

---

## Typical Usage Flow

1. Modify or add SnapLogic pipeline exports (`.slp`, `.slt`)
2. Commit changes to a branch
3. Open a Pull Request
4. GitHub Actions runs the CI/CD workflow
5. Merge to `main`
6. Assets are promoted to the staging project space

---

## Manual Workflow Execution

1. Go to **GitHub → Actions**
2. Select **SnapLogic CI/CD Demo**
3. Click **Run workflow**

---

## Future Enhancements (Optional)

Planned improvements for a full CI/CD lifecycle:

- Automated pipeline testing
- Validation tasks
- Cleanup of temporary test projects
- Promotion from staging to production
- Version tagging and release workflows

---

## Security Notes

Before using this in production:

- Do not commit credentials to the repository
- Use a dedicated CI user in SnapLogic
- Limit permissions for the CI account
- Review ACL settings in the workflow
- Consider secrets vault or token-based auth

---

## License

Add a `LICENSE` file if this repository will be reused or shared publicly.
