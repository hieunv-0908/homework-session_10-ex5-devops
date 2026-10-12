\# Bài 5: Tự động hóa điều phối đa dịch vụ với Docker Compose \& File .env



\## 1. Cấu trúc thư mục

```text

homework/session\_10/ex5/

├── docker-compose.yml

├── .env.example

├── .gitignore

└── README.md





CLI:



Admin@Admin MINGW64 /d/baitap\_devops/baitap/ss10/b5

$ docker compose up -d

\[+] up 2/2

&#x20;✔ Container db-server     Started                                          0.8s

&#x20;✔ Container app-container Started                                          0.9s



What's next:

&#x20;   Filter, search, and stream logs from all your Compose services

&#x20;   in one place with Docker Desktop's Logs view. docker-desktop://dashboard/logs?appId=b5



Admin@Admin MINGW64 /d/baitap\_devops/baitap/ss10/b5

$ docker compose ps

NAME            IMAGE                COMMAND                  SERVICE    CREATED              STATUS              PORTS

app-container   busybox              "sleep infinity"         backend    About a minute ago   Up About a minute

db-server       postgres:15-alpine   "docker-entrypoint.s…"   database   About a minute ago   Up About a minute   5432/tcp



Admin@Admin MINGW64 /d/baitap\_devops/baitap/ss10/b5

$ docker compose logs -f database

db-server  | The files belonging to this database system will be owned by user "postgres".

db-server  | This user must also own the server process.

db-server  |

db-server  | The database cluster will be initialized with locale "en\_US.utf8".

db-server  | The default database encoding has accordingly been set to "UTF8".

db-server  | The default text search configuration will be set to "english".

db-server  |

db-server  | Data page checksums are disabled.

db-server  |

db-server  | fixing permissions on existing directory /var/lib/postgresql/data ... ok

db-server  | creating subdirectories ... ok

db-server  | selecting dynamic shared memory implementation ... posix

db-server  | selecting default max\_connections ... 100

db-server  | selecting default shared\_buffers ... 128MB

db-server  | selecting default time zone ... UTC

db-server  | creating configuration files ... ok

db-server  | running bootstrap script ... ok

db-server  | sh: locale: not found

db-server  | 2026-10-12 21:09:48.815 UTC \[35] WARNING:  no usable system locales were found

db-server  | performing post-bootstrap initialization ... ok

db-server  | syncing data to disk ... ok

db-server  |

db-server  |

db-server  | Success. You can now start the database server using:

db-server  |

db-server  |     pg\_ctl -D /var/lib/postgresql/data -l logfile start

db-server  |

db-server  | initdb: warning: enabling "trust" authentication for local connections

db-server  | initdb: hint: You can change this by editing pg\_hba.conf or using the option -A, or --auth-local and --auth-host, the next time you run initdb.

db-server  | waiting for server to start....2026-10-12 21:09:50.544 UTC \[41] LOG:  starting PostgreSQL 15.19 on x86\_64-pc-linux-musl, compiled by gcc (Alpine 15.2.0) 15.2.0, 64-bit

db-server  | 2026-10-12 21:09:50.554 UTC \[41] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"

db-server  | 2026-10-12 21:09:50.582 UTC \[44] LOG:  database system was shut down at 2026-10-12 21:09:49 UTC

db-server  | 2026-10-12 21:09:50.597 UTC \[41] LOG:  database system is ready to accept connections

db-server  |  done

db-server  | server started

db-server  | CREATE DATABASE

db-server  |

db-server  |

db-server  | /usr/local/bin/docker-entrypoint.sh: ignoring /docker-entrypoint-initdb.d/\*

db-server  |

db-server  | waiting for server to shut down....2026-10-12 21:09:50.773 UTC \[41] LOG:  received fast shutdown request

db-server  | 2026-10-12 21:09:50.798 UTC \[41] LOG:  aborting any active transactions

db-server  | 2026-10-12 21:09:50.803 UTC \[41] LOG:  background worker "logical replication launcher" (PID 47) exited with exit code 1

db-server  | 2026-10-12 21:09:50.803 UTC \[42] LOG:  shutting down

db-server  | 2026-10-12 21:09:50.811 UTC \[42] LOG:  checkpoint starting: shutdown immediate

db-server  | 2026-10-12 21:09:51.011 UTC \[42] LOG:  checkpoint complete: wrote 922 buffers (5.6%); 0 WAL file(s) added, 0 removed, 0 recycled; write=0.059 s, sync=0.114 s, total=0.208 s; sync files=301, longest=0.052 s, average=0.001 s; distance=4249 kB, estimate=4249 kB

db-server  | 2026-10-12 21:09:51.018 UTC \[41] LOG:  database system is shut down

db-server  |  done

db-server  | server stopped

db-server  |

db-server  | PostgreSQL init process complete; ready for start up.

db-server  |

db-server  | 2026-10-12 21:09:51.115 UTC \[1] LOG:  starting PostgreSQL 15.19 on x86\_64-pc-linux-musl, compiled by gcc (Alpine 15.2.0) 15.2.0, 64-bit

db-server  | 2026-10-12 21:09:51.116 UTC \[1] LOG:  listening on IPv4 address "0.0.0.0", port 5432

db-server  | 2026-10-12 21:09:51.116 UTC \[1] LOG:  listening on IPv6 address "::", port 5432

db-server  | 2026-10-12 21:09:51.130 UTC \[1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"

db-server  | 2026-10-12 21:09:51.148 UTC \[57] LOG:  database system was shut down at 2026-10-12 21:09:51 UTC

db-server  | 2026-10-12 21:09:51.161 UTC \[1] LOG:  database system is ready to accept connections







Admin@Admin MINGW64 /d/baitap\_devops/baitap/ss10/b5

$ docker exec -it app-container ping -c 2 database

PING database (172.21.0.2): 56 data bytes

64 bytes from 172.21.0.2: seq=0 ttl=64 time=0.767 ms

64 bytes from 172.21.0.2: seq=1 ttl=64 time=0.142 ms



\--- database ping statistics ---

2 packets transmitted, 2 packets received, 0% packet loss

round-trip min/avg/max = 0.142/0.454/0.767 ms



What's next:

&#x20;   Try Docker Debug for seamless, persistent debugging tools in any container or image → docker debug app-container

&#x20;   Learn more at https://docs.docker.com/go/debug-cli/



Admin@Admin MINGW64 /d/baitap\_devops/baitap/ss10/b5

$ docker compose down

\[+] down 3/3

&#x20;✔ Container app-container Removed                                          1.3s

&#x20;✔ Container db-server     Removed                                          0.4s

&#x20;✔ Network b5\_backend-net  Removed                                          0.3s



Admin@Admin MINGW64 /d/baitap\_devops/baitap/ss10/b5

$



