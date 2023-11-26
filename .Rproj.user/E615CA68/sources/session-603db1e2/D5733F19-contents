#' @import caret
#'
scale_data <- function(matrix,d){
  matrix <- predict(d$scaler, matrix[,d$genes])
}

#'
deseq_normalise <- function(matrix,d){
  pseudo_reference <- d[["keep"]][[2]]
  keep <- d$keep[[1]]
  ratio_to_ref  <- apply(matrix[,keep],1, function(x) x/pseudo_reference)
  sizeFactor  <- apply(ratio_to_ref,2, function(x) stats::median(x))
  matrix <- matrix/sizeFactor
  matrix <- log(matrix + 1)
  matrix
}

#' @import kernlab
#'
pred_clusters <- function(matrix,d){
  predictions <- lapply(d$models, function(model){
    predictions <- -1 * predict(model, newdata = matrix, type = "decision")
  })
  predictions <- do.call(cbind, predictions)
  colnames(predictions) <- names(d$models)
  predictions <- data.frame(predictions)
  predictions$prediction <- apply(predictions, 1, function(x) colnames(predictions)[which.max(x)])
  predictions$pass_cutoff <- apply(predictions[,1:length(d$models)], 1, function(x) max(x) > -0.04)
  predictions
}

#' Transcriptional AML cluster prediction
#'
#' Predict the clusters found in "ref paper".
#' @param matrix Sample x genes matrix
#' @return A dataframe with for each of the 17 clusters the distance to the decision boundary, the cluster prediction and if the distance to the boundary was higher than the cutoff. Rownames of the imput matrix will be set as rownames and under the column "sample_id".
#' @export
predict_AML_clusters <- function(matrix){
  if(!any(class(matrix) %in% "matrix")){
    stop("\n\n Matrix needs to be of class matrix")
  }
  if(ncol(matrix) != 60660){
    stop("\n\nMissing genes, did you follow the correct expression counting pipeline? \n\nOr do you need to transpose the matrix? \nExpected is samples on the rows and genes on the columns")
  }
  if(ncol(matrix) != 60660){
    stop("Error: missing genes, did you follow the correct counting pipeline?")
  }
  if(sum(d$genes %in% colnames(matrix), na.rm = T) != 2000){
    stop("\n\nMissing genes, did you follow the correct expression counting pipeline? \n\nOr do you need to transpose the matrix? \nExpected is samples on the rows and genes on the columns")
  }
  if(any(matrix<0)|!is.integer(matrix[1,1])){
    stop("\n\n Matrix contains negative values or non-integers. Uncorrected and non-normalised raw counts are expected")
  }

  cat("Normalising data...\n")
  X <- deseq_normalise(matrix,d)
  cat("Scaling data...\n")
  X <- scale_data(X, d)
  cat("Predicting clusters...\n")
  out <- pred_clusters(X, d)
  rownames(out) <- rownames(matrix)
  out$sample_id <- rownames(matrix)
  return(out)
}
