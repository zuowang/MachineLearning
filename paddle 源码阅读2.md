//this = scaleAB*(a*b) + scaleT*this
Matrix::mul(const MatrixPtr a, const MatrixPtr b, real scaleAB, real scaleT)

以下更新weight的梯度：WGrad = input_T * oGrad + WGrad
weights_[i]->getWGrad()->mul(input_T, oGrad, 1, 1);

令u = input_T，v = oGrad，将（u，v）放入local_sv_queues_

