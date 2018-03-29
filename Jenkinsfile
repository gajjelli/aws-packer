#!groovy

node {

  def err = null
  currentBuild.result = "SUCCESS"

  try {
    stage 'Checkout.'
      checkout scm

    stage 'Validate.'
      def packer_file = 'ami-build-packer.json'
      print "Running packer validate on : ${packer_file}"

    /* ../packer build --var-file cred.json first_example.json */
    sh "/usr/local/packer -v ;/usr/local/packer validate ${packer_file}"

    stage 'Build'
/*    sh "/usr/local/packer build -var 'aws_Access_key=$AWS_ACCESS_KEY_ID' -var 'aws_secret_key=$AWS_SECRET_ACCESS_KEY' ${packer_file}" */
      sh "/usr/local/packer build --var-file cred.json ${packer_file}"	
    stage 'Test'
      print "Testing goes here."
  }

  catch (caughtError) {
    err = caughtError
    currentBuild.result = "FAILURE"
  }

  finally {
    /* Must re-throw exception to propagate error */
    if (err) {
      throw err
    }
  }
}
