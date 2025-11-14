# Zellij Workflow Automation Examples

Complete real-world workflow examples for various use cases.

## Software Development

### Full-Stack Web Application

**Scenario**: Working on a React frontend with Node.js/Express backend and PostgreSQL database.

```kdl
layout {
    // Define reusable templates
    default_tab_template {
        pane size=1 borderless=true {
            plugin location="zellij:tab-bar"
        }
        children
        pane size=2 borderless=true {
            plugin location="zellij:status-bar"
        }
    }
    
    pane_template name="dev-pane" split_direction="vertical" {
        pane size="65%"
        pane size="35%" start_suspended=true
    }
    
    // Frontend Tab
    tab name="Frontend" cwd="~/projects/myapp/frontend" focus=true {
        dev-pane {
            pane edit="src/App.tsx"
            pane command="npm" {
                args "run" "dev"
            }
        }
    }
    
    // Backend Tab
    tab name="Backend" cwd="~/projects/myapp/backend" {
        dev-pane {
            pane edit="src/server.ts"
            pane command="npm" {
                args "run" "dev"
            }
        }
    }
    
    // Database Tab
    tab name="Database" {
        pane split_direction="horizontal" {
            pane command="docker" {
                args "logs" "-f" "postgres"
            }
            pane  // For running psql queries
        }
    }
    
    // Tests Tab
    tab name="Tests" cwd="~/projects/myapp" {
        pane split_direction="horizontal" {
            pane cwd="frontend" command="npm" {
                args "test" "--" "--watch"
                start_suspended true
            }
            pane cwd="backend" command="npm" {
                args "test" "--" "--watch"
                start_suspended true
            }
        }
    }
    
    // Git Tab
    tab name="Git" cwd="~/projects/myapp" {
        pane split_direction="vertical" {
            pane command="git" {
                args "log" "--oneline" "--graph" "--all" "--decorate"
            }
            pane split_direction="horizontal" {
                pane  // For git commands
                pane command="git" {
                    args "status"
                }
            }
        }
    }
    
    // Custom keybindings for this workflow
    keybinds {
        shared {
            bind "Alt 1" { GoToTab 1; }
            bind "Alt 2" { GoToTab 2; }
            bind "Alt 3" { GoToTab 3; }
            bind "Alt 4" { GoToTab 4; }
            bind "Alt 5" { GoToTab 5; }
            bind "Alt r" { Run "npm" "run" "dev"; }
            bind "Alt t" { Run "npm" "test"; }
            bind "Alt g" { Run "git" "status"; }
        }
    }
}
```

### Microservices Architecture

**Scenario**: Multiple services, each with its own repository, running together.

```kdl
layout {
    default_tab_template {
        pane size=1 borderless=true {
            plugin location="zellij:tab-bar"
        }
        children
    }
    
    tab name="Gateway" cwd="~/services/api-gateway" focus=true {
        pane split_direction="vertical" {
            pane edit="src/gateway.ts" size="60%"
            pane split_direction="horizontal" size="40%" {
                pane command="npm" {
                    args "run" "dev"
                    start_suspended true
                }
                pane command="npm" {
                    args "run" "logs"
                    start_suspended true
                }
            }
        }
    }
    
    tab name="Auth Service" cwd="~/services/auth-service" {
        pane split_direction="vertical" {
            pane edit="src/auth.ts" size="60%"
            pane command="npm" {
                args "run" "dev"
                start_suspended true
            }
        }
    }
    
    tab name="User Service" cwd="~/services/user-service" {
        pane split_direction="vertical" {
            pane edit="src/users.ts" size="60%"
            pane command="npm" {
                args "run" "dev"
                start_suspended true
            }
        }
    }
    
    tab name="Payment Service" cwd="~/services/payment-service" {
        pane split_direction="vertical" {
            pane edit="src/payments.ts" size="60%"
            pane command="npm" {
                args "run" "dev"
                start_suspended true
            }
        }
    }
    
    tab name="Docker" {
        pane split_direction="horizontal" {
            pane command="docker-compose" {
                args "ps"
            }
            pane command="docker-compose" {
                args "logs" "-f"
            }
        }
    }
    
    tab name="Monitoring" {
        pane split_direction="horizontal" {
            pane command="watch" {
                args "-n" "2" "curl" "-s" "http://localhost:3000/health"
            }
            pane command="htop"
        }
    }
}
```

## DevOps and Infrastructure

### Kubernetes Cluster Management

```kdl
layout {
    tab name="Pods" focus=true {
        pane split_direction="vertical" {
            pane command="kubectl" {
                args "get" "pods" "-A" "-w"
            }
            pane split_direction="horizontal" {
                pane  // For kubectl commands
                pane command="kubectl" {
                    args "top" "pods" "-A"
                }
            }
        }
    }
    
    tab name="Deployments" {
        pane split_direction="horizontal" {
            pane command="kubectl" {
                args "get" "deployments" "-A" "-w"
            }
            pane command="kubectl" {
                args "get" "replicasets" "-A" "-w"
            }
        }
    }
    
    tab name="Services" {
        pane split_direction="horizontal" {
            pane command="kubectl" {
                args "get" "services" "-A"
            }
            pane command="kubectl" {
                args "get" "ingress" "-A"
            }
        }
    }
    
    tab name="Logs" {
        pane split_direction="vertical" {
            pane  // Pod selector
            pane  // Log viewer
        }
    }
    
    tab name="Events" {
        pane command="kubectl" {
            args "get" "events" "-A" "--sort-by=.lastTimestamp" "-w"
        }
    }
    
    tab name="Helm" {
        pane split_direction="horizontal" {
            pane command="helm" {
                args "list" "-A"
            }
            pane  // Helm commands
        }
    }
}
```

### Terraform Infrastructure

```kdl
layout {
    tab name="Code" cwd="~/infra/terraform" focus=true {
        pane split_direction="vertical" {
            pane size="20%" {
                plugin location="zellij:strider"
            }
            pane size="80%" split_direction="horizontal" {
                pane edit="main.tf" size="70%"
                pane size="30%"  // Terminal for terraform commands
            }
        }
    }
    
    tab name="Plan" cwd="~/infra/terraform" {
        pane split_direction="vertical" {
            pane command="terraform" {
                args "plan"
                start_suspended true
            }
            pane  // For reviewing plan output
        }
    }
    
    tab name="State" cwd="~/infra/terraform" {
        pane split_direction="horizontal" {
            pane command="terraform" {
                args "state" "list"
            }
            pane  // For state operations
        }
    }
    
    tab name="AWS Console" {
        pane split_direction="horizontal" {
            pane command="aws" {
                args "ec2" "describe-instances" "--output" "table"
            }
            pane  // AWS CLI commands
        }
    }
    
    keybinds {
        shared {
            bind "Alt p" { Run "terraform" "plan"; }
            bind "Alt a" { Run "terraform" "apply"; }
            bind "Alt d" { Run "terraform" "destroy"; }
        }
    }
}
```

### Docker Compose Development

```kdl
layout {
    tab name="Overview" focus=true {
        pane split_direction="horizontal" {
            pane command="docker-compose" {
                args "ps"
            }
            pane command="docker" {
                args "ps" "-a"
            }
        }
    }
    
    tab name="Logs" {
        pane split_direction="vertical" {
            pane command="docker-compose" {
                args "logs" "-f" "api"
            }
            pane command="docker-compose" {
                args "logs" "-f" "db"
            }
        }
    }
    
    tab name="Compose File" {
        pane edit="docker-compose.yml"
    }
    
    tab name="Stats" {
        pane command="docker" {
            args "stats"
        }
    }
    
    keybinds {
        shared {
            bind "Alt u" { Run "docker-compose" "up" "-d"; }
            bind "Alt d" { Run "docker-compose" "down"; }
            bind "Alt r" { Run "docker-compose" "restart"; }
        }
    }
}
```

## Data Science and ML

### Machine Learning Workflow

```kdl
layout {
    tab name="Notebook" cwd="~/ml-project" focus=true {
        pane split_direction="vertical" {
            pane edit="analysis.ipynb" size="70%"
            pane command="jupyter" {
                args "lab" "--no-browser"
                start_suspended true
            }
        }
    }
    
    tab name="Training" cwd="~/ml-project" {
        pane split_direction="horizontal" {
            pane command="python" {
                args "train.py"
                start_suspended true
            }
            pane command="tensorboard" {
                args "--logdir" "logs"
                start_suspended true
            }
        }
    }
    
    tab name="Data Prep" cwd="~/ml-project" {
        pane split_direction="vertical" {
            pane edit="preprocessing.py" size="60%"
            pane split_direction="horizontal" {
                pane  // Python REPL
                pane command="watch" {
                    args "-n" "5" "ls" "-lh" "data/"
                }
            }
        }
    }
    
    tab name="Monitoring" {
        pane split_direction="horizontal" {
            pane command="nvidia-smi" {
                args "-l" "2"
            }
            pane command="htop"
        }
    }
}
```

## Content Creation

### Technical Blog Writing

```kdl
layout {
    tab name="Editor" cwd="~/blog" focus=true {
        pane split_direction="vertical" {
            pane edit="posts/new-post.md" size="65%"
            pane split_direction="horizontal" size="35%" {
                pane command="hugo" {
                    args "server" "-D"
                    start_suspended true
                }
                pane  // Preview browser commands
            }
        }
    }
    
    tab name="Images" cwd="~/blog/static/images" {
        pane split_direction="horizontal" {
            pane  // Image editing
            pane command="ls" {
                args "-lh"
            }
        }
    }
    
    tab name="Research" {
        pane split_direction="vertical" {
            pane  // Browser for research
            pane edit="drafts/notes.md"  // Note taking
        }
    }
    
    tab name="Deploy" cwd="~/blog" {
        pane split_direction="horizontal" {
            pane command="git" {
                args "status"
            }
            pane  // Deployment commands
        }
    }
}
```

### Documentation Writing

```kdl
layout {
    tab name="Docs" cwd="~/docs" focus=true {
        pane split_direction="vertical" {
            pane size="20%" {
                plugin location="zellij:strider"
            }
            pane size="50%" edit="index.md"
            pane size="30%" split_direction="horizontal" {
                pane command="mdbook" {
                    args "serve"
                    start_suspended true
                }
                pane  // Live preview browser
            }
        }
    }
    
    tab name="API Reference" cwd="~/docs/api" {
        pane split_direction="horizontal" {
            pane edit="endpoints.md" size="60%"
            pane  // For testing API calls
        }
    }
    
    tab name="Build" cwd="~/docs" {
        pane split_direction="horizontal" {
            pane command="mdbook" {
                args "build"
                start_suspended true
            }
            pane command="mdbook" {
                args "test"
                start_suspended true
            }
        }
    }
}
```

## System Administration

### Server Monitoring

```kdl
layout {
    tab name="System" focus=true {
        pane split_direction="horizontal" {
            pane command="htop"
            pane split_direction="vertical" {
                pane command="watch" {
                    args "-n" "2" "df" "-h"
                }
                pane command="watch" {
                    args "-n" "2" "free" "-h"
                }
            }
        }
    }
    
    tab name="Network" {
        pane split_direction="horizontal" {
            pane command="iftop" {
                args "-i" "eth0"
            }
            pane command="ss" {
                args "-tunapl"
            }
        }
    }
    
    tab name="Logs" {
        pane split_direction="vertical" {
            pane command="tail" {
                args "-f" "/var/log/syslog"
            }
            pane command="journalctl" {
                args "-f" "-u" "nginx"
            }
        }
    }
    
    tab name="Processes" {
        pane split_direction="horizontal" {
            pane command="ps" {
                args "aux" "--sort=-pcpu"
            }
            pane  // For kill/nice commands
        }
    }
}
```

### Database Administration

```kdl
layout {
    tab name="Queries" cwd="~/sql" focus=true {
        pane split_direction="vertical" {
            pane edit="queries/analysis.sql" size="60%"
            pane command="psql" {
                args "-h" "localhost" "-U" "admin" "mydb"
            }
        }
    }
    
    tab name="Monitoring" {
        pane split_direction="horizontal" {
            pane command="watch" {
                args "-n" "5" "psql" "-c" "SELECT * FROM pg_stat_activity;"
            }
            pane command="watch" {
                args "-n" "5" "pg_top"
            }
        }
    }
    
    tab name="Backups" {
        pane split_direction="horizontal" {
            pane  // Backup commands
            pane command="ls" {
                args "-lh" "/backups/"
            }
        }
    }
    
    tab name="Logs" {
        pane command="tail" {
            args "-f" "/var/log/postgresql/postgresql.log"
        }
    }
}
```

## Personal Productivity

### Daily Planning Layout

```kdl
layout {
    tab name="Tasks" focus=true {
        pane split_direction="horizontal" {
            pane edit="~/notes/today.md" size="50%"
            pane edit="~/notes/inbox.md" size="50%"
        }
    }
    
    tab name="Calendar" {
        pane split_direction="vertical" {
            pane command="calcurse"
            pane edit="~/notes/meetings.md"
        }
    }
    
    tab name="Email" {
        pane command="neomutt"
    }
    
    tab name="Terminal" {
        pane
    }
}
```

### Research Session

```kdl
layout {
    tab name="Notes" cwd="~/research" focus=true {
        pane split_direction="horizontal" {
            pane edit="literature-review.md" size="60%"
            pane edit="references.bib" size="40%"
        }
    }
    
    tab name="Papers" cwd="~/research/papers" {
        pane split_direction="vertical" {
            pane  // PDF viewer
            pane edit="notes.md"
        }
    }
    
    tab name="Data" cwd="~/research/data" {
        pane split_direction="horizontal" {
            pane  // Data analysis
            pane command="watch" {
                args "-n" "5" "ls" "-lh"
            }
        }
    }
}
```

## Tips for Creating Workflows

1. **Start with your actual workflow**: Map your real-world tasks first
2. **Use tabs for contexts**: Each tab should represent a distinct work context
3. **Group related panes**: Keep related information together in split panes
4. **Leverage CWD composition**: Set base directories at tab level
5. **Add custom keybindings**: Include common commands as Alt+key bindings
6. **Use start_suspended for commands**: Prevents overwhelming system on startup
7. **Include monitoring where relevant**: Add resource monitoring for long-running processes
8. **Test incrementally**: Build layout piece by piece
9. **Document with comments**: Add KDL comments explaining sections
10. **Version control your layouts**: Store layouts in git with your dotfiles
