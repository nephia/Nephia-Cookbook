# About

This is FAQ. If you want to raise a debate, please create a new issue in this repository.

## Developing a plugin

### Can I pass some parameters to plugin?

Yes, you can. See following example.

    # in your app class
    package Your::App;
    use Nephia plugins => [
        SomePlugin => { foo => 'bar' },
    ];

    # in SomePlugin.pm
    package Nephia::Plugin::SomePlugin;
    use parent 'Nephia::Plugin';
    sub new {
        my ($class, %opts) = @_; # %opts is ( foo => 'bar')
        my $self = $class->SUPER::new(%opts);
        # some initialize logic here
        return $self;
    }


### How to call DSL that provided by other plugin from internal of plugin?

use "dsl" method that provided by Nephia::Core. See following example.

    package Nephia::Plugin::MyCleverOne;
    use strict;
    use parent 'Nephia::Plugin';
    
    sub some_method {
        my $self = shift;
        my $app  = $self->app              # subclass of Nephia::Core
        my $code = $app->dsl('some_dsl');  # $code is coderef of DSL "some_dsl"
        $code->(...);
        ...
    }

