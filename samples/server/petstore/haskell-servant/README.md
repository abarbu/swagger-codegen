# Auto-Generated Swagger Bindings to `SwaggerPetstore`

The library in `lib` provides auto-generated-from-Swagger bindings to the SwaggerPetstore API.

## Installation

Installation follows the standard approach to installing Stack-based projects.

1. Install the [Haskell `stack` tool](http://docs.haskellstack.org/en/stable/README).
2. Run `stack install` to install this package.

## Main Interface

The main interface to this library is in the `SwaggerPetstore.API` module, which exports the SwaggerPetstoreBackend type. The SwaggerPetstoreBackend
type can be used to create and define servers and clients for the API.

## Creating a Client

A client can be created via the `createSwaggerPetstoreClient` function. Clients can be run with runSwaggerPetstoreClient.
This function takes a bool, is the connection going to be plain or TLS, the hostname, and the port to connect to.

```haskell
{-# LANGUAGE RecordWildCards #-}

import SwaggerPetstore.API

main :: IO ()
main = do
  let SwaggerPetstoreBackend{..} = createSwaggerPetstoreClient
  -- Any SwaggerPetstore API call can go where the return below is. These are executed in the ClientM monad from Servant.
  a <- runSwaggerPetstoreClient (ServerConfig False "localhost" 8080) (return ())
  return ()
```

Note that the client will attempt to follow 301 Moved Permanently responses from
the server. This can lead to very poor performance. Best to find these redirects
and directly put in the correct endpoint address.

## Creating a Server

In order to create a server, you must use the `runSwaggerPetstoreServer` function. However, you unlike the client, in which case you *got* a `SwaggerPetstoreBackend`
from the library, you must instead *provide* a `SwaggerPetstoreBackend`. For example, if you have defined handler functions for all the
functions in `SwaggerPetstore.Handlers`, you can write:

```haskell
{-# LANGUAGE RecordWildCards #-}

import SwaggerPetstore.API

-- A module you wrote yourself, containing all handlers needed for the SwaggerPetstoreBackend type.
import SwaggerPetstore.Handlers

-- Run a SwaggerPetstore server on localhost:8080
main :: IO ()
main = do
  let server = SwaggerPetstoreBackend{..}
  runSwaggerPetstoreServer (ServerConfig "localhost" 8080) server
```

You could use `optparse-applicative` or a similar library to read the host and port from command-line arguments:
```
{-# LANGUAGE RecordWildCards #-}

module Main (main) where

import SwaggerPetstore.API (runSwaggerPetstoreServer, SwaggerPetstoreBackend(..), ServerConfig(..))

import Control.Applicative ((<$>), (<*>))
import Options.Applicative (execParser, option, str, auto, long, metavar, help)

main :: IO ()
main = do
  config <- parseArguments
  runSwaggerPetstoreServer config SwaggerPetstoreBackend{}

-- | Parse host and port from the command line arguments.
parseArguments :: IO ServerConfig
parseArguments =
  execParser $
    ServerConfig
      <$> option str  (long "host" <> metavar "HOST" <> help "Host to serve on")
      <*> option auto (long "port" <> metavar "PORT" <> help "Port to serve on")
```
