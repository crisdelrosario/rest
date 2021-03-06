# rest
--
    import "."


## Usage

```go
const (
	// NewInstanceMsg sets the message to indicate the start of the log
	NewInstanceMsg = "START"
	// EndInstanceMsg sets the message to indicate the end of the log
	EndInstanceMsg = "END"
	// LogLevelDebug defines a normal debug log
	LogLevelDebug = "DEBUG"
	// LogLevelPanic defines a panic log
	LogLevelPanic = "PANIC"
	// LogLevelFatal defines a fatal log
	LogLevelFatal = "FATAL"
)
```

```go
const (
	// ContentTypeJSON defines Content-Type for application/json
	ContentTypeJSON = "application/json"
	// ContentTypeTextPlain defines Content-Type for text/plain
	ContentTypeTextPlain = "text/plain"
)
```

```go
const (
	// ServerEnvDev defines the DEVELOPMENT environment
	ServerEnvDev = "DEVELOPMENT"
	// ServerEnvTesting defines the TESTING environment
	ServerEnvTesting = "TESTING"
)
```

```go
const (
	// ConfigExt defines the configuration extention that can be used
	ConfigExt = ".yml"
)
```

```go
var (
	// EmptyHandler creates an empty pass through handler
	EmptyHandler = func(r *mux.Router) http.Handler { return r }
)
```

#### func  ApplyRoutes

```go
func ApplyRoutes(router *mux.Router, routes Routes, s *Server) *mux.Router
```
ApplyRoutes set the Routes given the array of route

#### func  LoadConfig

```go
func LoadConfig(path string, out interface{}) error
```
LoadConfig creates a new instance of configuration from a file

#### type Config

```go
type Config struct {
}
```

Config represents information about a rest config.

#### func (Config) NewTempFile

```go
func (c Config) NewTempFile(text string) (*os.File, string)
```
NewTempFile creates a configuration file

#### type Entry

```go
type Entry struct {
	Level   string
	Message string
	Time    time.Time
}
```

Entry represents information about a rest server log entry.

#### type Log

```go
type Log struct {
	Entry []Entry
}
```

Log represents information about a rest server log.

#### func  NewLog

```go
func NewLog() Log
```
NewLog creates new instance of Log

#### func (*Log) Dump

```go
func (l *Log) Dump()
```
Dump will print all the messages to the io.

#### func (*Log) Fatal

```go
func (l *Log) Fatal(v ...interface{})
```
Fatal is equivalent to Print() and followed by a call to os.Exit(1)

#### func (*Log) Panic

```go
func (l *Log) Panic(v ...interface{})
```
Panic then throws a panic with the same message afterwards

#### func (*Log) Print

```go
func (l *Log) Print(v ...interface{})
```
Print a regular log

#### type Resource

```go
type Resource struct {
	Vars     map[string]string
	Response http.ResponseWriter
	Request  *http.Request
	Log      *Log
	Route    Route
}
```

Resource represents the information about the Resource.

#### func (*Resource) Deinit

```go
func (c *Resource) Deinit()
```
Deinit method that finalizes the Resource.

#### func (*Resource) Delete

```go
func (c *Resource) Delete()
```
Delete represents http.delete

#### func (*Resource) Get

```go
func (c *Resource) Get()
```
Get represents http.get

#### func (*Resource) Init

```go
func (c *Resource) Init() bool
```
Init method that initialized the Resource. Returning false will skip executing
the method and proceed to deinit()

#### func (*Resource) Patch

```go
func (c *Resource) Patch()
```
Patch represents http.patch

#### func (*Resource) Post

```go
func (c *Resource) Post()
```
Post represents http.post

#### func (*Resource) Put

```go
func (c *Resource) Put()
```
Put represents http.put

#### func (*Resource) Set

```go
func (c *Resource) Set(vars map[string]string, w http.ResponseWriter, r *http.Request, l *Log, rt Route)
```
Set method to set the following properties

#### func (*Resource) SetContentType

```go
func (c *Resource) SetContentType(ctype string)
```
SetContentType method to set the content type

#### func (*Resource) SetStatus

```go
func (c *Resource) SetStatus(code int)
```
SetStatus method to set the header status code

#### type ResourceType

```go
type ResourceType interface {
	Set(map[string]string, http.ResponseWriter, *http.Request, *Log, Route)

	Init() bool

	Get()

	Put()

	Post()

	Patch()

	Delete()

	Deinit()
}
```

ResourceType represents an interface information about a rest resource.

#### type Route

```go
type Route struct {
	Name     string
	Pattern  string
	Resource ResourceType
	Server   *Server
}
```

Route represents the struct of Route

#### func  NewRoute

```go
func NewRoute(n string, p string, r ResourceType) Route
```
NewRoute creates a new route

#### func (Route) GetHandler

```go
func (route Route) GetHandler(s *Server) func(http.ResponseWriter, *http.Request)
```
GetHandler is the method that handles the http.HandlerFunc

#### func (Route) GetSimplePattern

```go
func (route Route) GetSimplePattern() string
```
GetSimplePattern returns the pattern without the regex rules

#### type Routes

```go
type Routes []Route
```

Routes represents a array/collection of Route

#### type Server

```go
type Server struct {
	Port        string
	Environment string
	AccessLog   string

	AccessLogFile *os.File
	Router        *mux.Router
}
```

Server represents information about a rest server.

#### func (*Server) Listen

```go
func (server *Server) Listen(h func(*mux.Router) http.Handler)
```
Listen initiates the handlers

#### func (*Server) Routes

```go
func (server *Server) Routes(r Routes, def func(http.ResponseWriter, *http.Request), router *mux.Router)
```
Routes sets up the configuration of the server and creates an instance
