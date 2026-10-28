# Project 10: Secure Digital Shopping Ledger (S4 System)
# Run with: shiny::runApp()

library(shiny)

# ---------- S4 CLASS WITH EXPLICIT SLOTS + VALIDATOR ----------
setClass(
  "Wallet",
  representation(
    owner   = "character",
    balance = "numeric",
    history = "data.frame"
  ),
  validity = function(object) {
    if (length(object@balance) != 1 || is.na(object@balance)) {
      return("S4 Validation Failure: Balance must be a single number.")
    }
    if (object@balance < 0) {
      return("S4 Validation Failure: Balance cannot be negative.")
    }
    TRUE
  }
)

# ---------- GENERICS + METHODS ----------
setGeneric("purchase", function(wallet, item, price, qty) standardGeneric("purchase"))
setGeneric("addFunds", function(wallet, amount) standardGeneric("addFunds"))

setMethod("purchase", "Wallet", function(wallet, item, price, qty) {
